# Play

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

### Headers and Cookies





























































