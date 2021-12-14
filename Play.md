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



































































