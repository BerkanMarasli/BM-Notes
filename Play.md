# Play

<br>

https://books.underscore.io/essential-play/essential-play.html#constructing-results



<br>

The backbone of a Play web application is made up of `Actions`, `Controllers`, and routes:

- `Actions` are functions from `Requests` to `Results`
- `Controllers` are collections of action-producing methods
- Routes map incoming `Requests` to Action-producing method calls on our `Controllers`

We typically place controllers in a `Controllers` package in the app/controllers folder.
Routes go in the conf/routes file (no filename extension).

<br>





## Routes

### Path Parameters

```
# Fixed route (no parameters):
GET /hello controllers.HelloController.hello

# Single parameter:
GET /hello/:name controllers.HelloController.helloTo(name: String)

# Multiple parameters:
GET /send/:msg/to/:user controllers.ChatController.send(msg: String, user: String)

# Rest-style parameter:
GET /download/*filename controllers.DownloadController.file(filename: String)
```

Play supports `OPTIONS`, `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `TRACE`, and `CONNECT`.

Play routing is strict (hence, no trailing slash will match). We can overcome this by mapping multiple routes to a single method call: `GET /hello` and `GET /hello/` to the same method call.

<br>

### Query Parameters

We can specify parameters in the method-call section of a route without declaring them in the URI. When we do this Play extracts the values from the query string instead:

```
# Extract 'username' and 'message' from the path:
GET /send/:message/to/:username controllers.ChatController.send(message: String, username: String)

# Extract 'username' and 'message' from the query string:
GET /send controllers.ChatController.send(message: String, username: String)

# Extract 'username' from the path and 'message' from the query string:
GET /send/to/:username controllers.ChatController.send(message: String, username: String)
^ GET /send/to/berkan?message=hello -> controllers.ChatController.send("hello", "berkan")
```

We can make query string parameters optional by defining them as `Option` types.
Play will pass `Some(value)` if the URI contains the parameter and `None` if it does not.
Note: path parameters are always required.

```scala
object NotificationController extends Controller {
  def notify(username: String, message: Option[String]) =
  	Action { request => /* ... */}
}
```

```
GET /notify controllers.NotificationController.notify(username: String, message: Option[String])
```

<br>

### Typed Parameters

We can extract path and query parameters of types other than String.
This allows us to define Actions using well-typed arguments without messy parsing code.
Play has built-in support for `Int`, `Double`, `Long`, `Boolean`, and `UUID` parameters.

```
GET /say/:msg/:n/times controllers.VerboseController.say(msg: String, n: Int)
```

```scala
object VerboseController extends Controller {
  def say(msg: String, n: Int) = Action { request =>
  	Ok(List.fill(n)(msg) mkString "\n")
  }
}
```

```
/say/Hello/5/times
Hello
Hello
Hello
Hello
Hello
```

Play also has built-in support for `Option` and `List` parameters in the query string (but no in the path):

```
GET /option-example controllers.MyController.optionExample(arg: Option[Int])
GET /list-example controllers.MyController.listExample(arg: List[Int])
```

```
/option-example # => MyController.optionExample(None)
/option-example?arg=123 # => MyController.optionExample(Some(123))
/list-example # => MyController.listExample(Nil)
/list-example?arg=123 # => MyController.listExample(List(123))
/list-example?arg=12&arg=34 # => MyController.listExample(List(12,34))
```

<br>

### Reverse Routing

Play also generates *reverse routes* that map method calls back to URIs. These are placed in a synthetic `routes` package that we can access from our Scala code.

<br>





## Parsing Requests

Clients can `POST` or `PUT` data in a huge range of formats, the most common being JSON, XML , and form data.
`Request` is a generic type, `Request[A]` which indicates the type of body that cna be retrieved via the `body` method:

```scala
def index = Action { request =>
  val body: ??? = request.body
}
```

Play cannot know the `Content-Type` of a request at compile time. Therefore, by default our actions handle request of type `Request[AnyContent]`.
[play.api.mvc.AnyContent](https://www.playframework.com/documentation/2.3.x/api/scala/index.html#play.api.mvc.AnyContent) is a sealed trait with subtypes for several common content types and a set of convenience methods that return `Some` if the request matches the relevant type and `None` if it does not:

| Method of `AnyContent` | Request content type              | Return type                      |
| ---------------------- | --------------------------------- | -------------------------------- |
| asText                 | text/plain                        | Option[String]                   |
| asFormUurlEncoded      | application/x-www-form-urlencoded | Option[Map[String, Seq[String]]] |
| AsMultipartFormData    | multipart/form-data               | Option[MultipartFormData]        |
| asJson                 | application/json                  | Option[JsValue]                  |
| asXml                  | application/xml                   | Option[NodeSeq]                  |
| asRaw                  | any other content type            | Option[RawBuffer]                |

```scala
def exampleAction = Action { request =>
  request.body.asJson match {
    case Some(json) => // handle Json
    case None => BadRequest("That's no Json!")
  }
}
```

We can alternatively implement handlers for multiple content types and chain them together with calls to `map`, `flatmap`, `orElse`, and `getOrElse`:

```scala
def exampleAction2 = Action { request =>
	(request.body.asText map handleText) orElse
  (request.body.asJson map handleJson) orElse
  (request.body.asXML map handleXML) getOrElse
}
def handleText(data: String): Result = ???
def handleJson(data: JsValue): Result = ???
def handleXml(data: NodeSeq): Result = ???
```

<br>

#### Custom Body Parsers

if we are certain about the data type we want in a particular Action, we can specify a body parser to restrict it to a specific type. Play returns a 400 Bad Request response to the client if it cannot parse the request as the relevant type:

```scala
import play.api.mvc.BodyParsers.parse

def index = Action(parse.json) { request =>
	val body: JsValue = request.body
  // ... no need to call 'body.asJson' ...
}
```

We can even implement our own custom body parsers to parse exotic formats.

<br>

### Headers and Cookies

`Request` contains two methods for inspecting HTTP headers:

- the `headers` method returns a [play.api.mvc.Headers](https://www.playframework.com/documentation/2.3.x/api/scala/index.html#play.api.mvc.Headers) object for inspecting general headers
- the `cookies` method returns a [play.api.mvc.Cookies](https://www.playframework.com/documentation/2.3.x/api/scala/index.html#play.api.mvc.Cookies) object for inspecting the `Cookies` header

Values are treated as `Strings` throughout. Play doesn't attempt to parse headers as dedicated Scala types.

```scala
object RequestDemo extends Controller {
  def headers = Action { request =>
  	val headers: Headers = request.headers
    val ucType: Option[String] = headers.get("Content-Type")
    val lcType: Option[String] = headers.get("content-type")
    
    val cookies: Cookies = request.cookies
    val cookie: Option[Cookie] = cookies.get("DemoCookie")
    val value: Option[String] = cookie.map(_.value)
    
    Ok(Seq(
      s"Headers: $headers",
      s"Content-Type: $ucType",
      s"content-type: $lcType",
      s"Cookies: $cookies",
      s"Cookie value: $value"
    ) mkString "\n")
  }
}
```

Note:

- `Headers.get` method is case insensitive
- Cookie names are case sensitive

### Methods and URIs

The `Request` object also provides methods that are of occasional use:

```scala
// The HTTP method ("GET", "POST", etc)
val method: String = request.method

// The URI, including path and query string
val uri: String = request.uri

// The path of the URI, without the query string
val path: String = request.path

// The query string, split into name/value pairs:
val query: Map[String, Seq[String]] = request.queryString
```

<br>





## Constructing Results

Create `Results` (final step of any Action), populate them with content, and add headers and cookies.

### Status Code

Play provides a convenient set of factory objects for creating `Results`. These are defined in the [play.api.mvc.Results](https://www.playframework.com/documentation/2.3.x/api/scala/index.html#play.api.mvc.Results) trait and inherited by [play.api.mvc.Controller](https://www.playframework.com/documentation/2.3.x/api/scala/index.html#play.api.mvc.Controller):

| Constructor         | HTTP status code                   |
| ------------------- | ---------------------------------- |
| Ok                  | 200 Ok                             |
| NotFound            | 404 Not Found                      |
| InternalServerError | 500 Internal Server Error          |
| Unauthorised        | 401 Unauthorised                   |
| Status(number)      | number (an Int) - anything we want |

```scala
val result1: Result = Ok("Success!")
val result2: Result = NotFound("Cannot be found!")
val result3: Result = Status(401)("Access denied!")
```

<br>

### Adding Content

Play adds `Content-Type` headers to our `Results` based on the type of data we provide.
String -> text/plain
play.api.libs.json.JsValue -> application/json

The process of creating a `Result` is type-safe and Play determines the method of serialisation based on the type we give it.
As a consequence the final steps in an `Action` tend to be as follows:

- Convert the result of action to a type that Play can serialise
- Use the serialisable data to create a Result
- Tweak HTTP headers and so on.
- Return the Result

#### Custom Result Types

```scala
// We have a custom library for manipulating iCal calendar files:
case class ICal(/* ... */)

// We implement an implicit `Writeable[ICal]`:
implicit object ICalWriteable extends Writeable[ICal] {
  // ...
}

// Now our actions can serialize `ICal` results:
def action = Action { request =>
  val myCal: ICal = ICal(/* ... */)

  Ok(myCal) // Play uses `ICalWriteable` to serialize `myCal`
}
```

<br>

### Tweaking the Result

- We can cahnge the `Content-Type` header (without changing the content) using the `as` method
- We can add and/or alter HTTP headers using `withHeaders`
- We can add and/or alter cookies using `withCookies`

These methods can be chained, allowing us to create the Result, tweak it, and return it in a single expression:

```scala
def exampleAction = Action { request =>
	Ok("Success").
  as("text/plain").
  withHeaders(
  	"Cache-Control" -> "no-cache, no-store, must-revalidate",
    "Pragma" -> "no-cache",
    "Expires" -> "0",
    // etc..
  ).
  withCookies(
  	Cookie(name = "DemoCookie", value = "DemoCookieValue"),
    Cookie(name = "OtherCookie", value = "OtherCookieValue")
  )
}
```

<br>



## Handling Failure





























































