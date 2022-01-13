# Play

<br>

https://books.underscore.io/essential-play/essential-play.html#constructing-results

<br>

[TOC]

<br>

# Play Basics

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



# Working with JSON

Play ships with a built-in library for reading, writing, and manipulating JSON data, unsurpisingly called `play-json`.

<br>

## Modelling JSON

![](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.13.54.png)

- `Json.arr(...)` creates a JsArray. The method takes any number of parameters, each of which must be a JsValue or a type that can be implicitly converted to one.
- Json.obj(...) creates a JsObject. The method takes any number of parameters, each of which must be a pair of a string and a JsValue or convertible.

![Screenshot 2022-01-13 at 01.17.25](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.17.25.png)

### JSON Requests and Results

If writing an API endpoint that must accept JSON, use the built-in JSON body parser to receive a `Request[JsValue]` instead. Play will respond with a 400 Bad Request result if the reqiest does not contain JSON:

![Screenshot 2022-01-13 at 01.22.45](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.22.45.png)

![Screenshot 2022-01-13 at 01.23.12](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.23.12.png)

As Play extracts JsValues from incoming Requests, typically you do not directly parse stringified JSON. However, if you do:

![Screenshot 2022-01-13 at 01.26.56](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.26.56.png)

And the opposite is `Json.stringify`:

![Screenshot 2022-01-13 at 01.27.40](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.27.40.png)

### Deconstructing and Traversing JSON Data

#### Pattern Matching

One way of deconstructing JsValues is to use pattern matching. This is convenient as the subtypes are all case classes and case objects:

![Screenshot 2022-01-13 at 01.31.20](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.31.20.png)

#### Traversal Methods

Pattern matching only gets us so far. We can't easily search through the children of a JsObject or JsArray without looping. Fortunately, JsValue contains three methods to drill down to specific fields before we match:

- `json \ "nameOfField"` extracts a field from json assuming (a) json is a JsObject and (b) the field "nameOfField" exists
- `json(index)` extracts a field from json assuming (a) json is a JsArray and (b) the index exists
- `json \\ "nameOfField"`  extracts all fields named "nameOfField" from json and any of its descendents

![Screenshot 2022-01-13 at 01.38.00](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.38.00.png)

The subtype JsUndefined is used by Play to represent the failure to find a field. In the example, the expression to calculate z actually fails twice: first at the call to apply(2) and second at the call to \ "name". The implementation of JsUndefined carries the errors over into the final result:

![Screenshot 2022-01-13 at 01.41.33](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.41.33.png)

#### Parsing Methods

Can use two methods, `as` and `asOpt` to convert JSON data into regular Scala types.

The `as` method is most useful when exploring JSON in the REPL or unit tests:

![Screenshot 2022-01-13 at 01.44.49](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.44.49.png)

If `as` cannot convert the data to the type requested, it throws a run-time exception (dangerous for production).

The `asOpt` method provides a safer way to extract data - it attempts to parse the JSON as the desired type, and returns `None` if it fails. This is a better choice for use in application code:

![Screenshot 2022-01-13 at 01.46.50](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.46.50.png)

#### All Together

Traversal and pattern matching are complimentary techniques for dissecting JSON data. We can extract specific fields using traversal, and pattern match on them to extract Scala values:

![Screenshot 2022-01-13 at 01.49.19](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 01.49.19.png)

<br>

## Writing JSON

The application code in a typical REST API operates on a domain model consisting of sealed traits and case classes. When we have finished an operation, we have to take the result, convert it to JSON, and wrap it in a Result to send it to the client.

#### Writes

We convert Scala values to JSON using the Play Writes trait:

```scala
trait Writes[A] {
  def writes(value: A): JsValue
}
```

Play provides built-in Writes for many standard data tyoes, and we can create Writes by hand for any data type we want to serialise.

Play also provides a simple one-liner of defining a Writes for a case class:

```scala
case class Address(number: Int, street: String)

val addressWrites: Writes[Address] = Json.writes[Address]
```

Json.writes is a macro that inspects the Address type and generates code to define a sensible Writes. The macro saves us some typing but it only works on case classes.

addressWrites is an object that can serialise an Address to a JsObject with sensible field names and values:

![Screenshot 2022-01-13 at 03.04.54](/Users/berkanmarasli/Desktop/Screenshot 2022-01-13 at 03.04.54.png)

#### Implicit Writes













<br>

## Reading JSON



<br>

## JSON Formats



<br>

## Custom Formats



<br>























































