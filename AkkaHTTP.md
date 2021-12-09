# Akka HTTP

<br>

## Basic Syntax

### Example

```scala
val host: String = "localhost"
val port: Int = 7000
implicit val system = ActorSystem(Behaviors.empty, "HTTP_SERVER")
implicit val materializer : ActorMaterializer.type = ActorMaterializer

val topLevelRoute: Route =
    concat(
      pathSingleSlash {
        get {
          complete(HttpEntity(ContentTypes.`text/html(UTF-8)`, "<h1>Path = /</h1>"))
        }
      },
      path("hello") {
        get {
          complete("Path = /hello")
        }
      },
      path("crash") {
        get {
          complete("Path = /crash")
        }
      },
      pathPrefix("abc")(abcRoutes)
    )

val abcRoutes: Route =
    concat(
    path("berkan") {
      get {
        complete("Path = /abc/berkan")
      }
    },
    path("serkan") {
      get {
        complete("Path = /abc/serkan")
      }
    },
    get {
      complete("Path = /abc/(with no berkan/serkan)")
    }
    )

val binding: Future[ServerBinding] = Http().newServerAt(host, port).bind(topLevelRoute)

binding.onComplete {
    case Success(value) =>
      println(s"Server is listening on http://$host:${port}")
    case Failure(exception) =>
      println(s"Failure : $exception")
      system.terminate()
}
```

```scala
trait JsonSupport extends SprayJsonSupport with DefaultJsonProtocol {
  implicit val emailFormat = jsonFormat2(EmailAccount)
  // EmailAccount = class you want to format??
}
```



### Handling `get`s

```scala
// Parameters in query string

path("pathName") {
  parameters("para1", "para2", ...) { (para1, para2, ...) =>
  	complete(s"$para1 + $para2")
  }
}
// /pathName?para1=XXX&para2=YYY

path("pathName") {
  parameters("para1", "para2".optional) { (para1, para2, ...) =>
  	complete(s"$para1 + ${para2.getOrElse("not sent")}")
  }
}
// /pathName?para1=XXX
// para1 = "XXX", para2 = "not sent"
```

```scala
// 

path("pathName" / IntNumber) { i =>
  complete(s"$i")
}
// /pathName/4

path("pathName" / Segment) { i =>
  complete(s"$i")
}
// /pathName/xxx

path("pathName" / RemainingPath) { i =>
  complete(s"$i")
}
// /pathName/xxx/yyy/zzz
// gives xxx/yyy/zzz
```



### Handling `post`s

```scala
post {
  entity(as[type]) { account =>
    complete()
  }
}
// type can be String, case class e.g. EmailAccount
```











<br>

## Routing

```
When a route receives a request, it can do one of these things:
- complete() = a given response is sent to client as reaction to the request
- reject() = the route does not want to handle the request
- fail()
- redirect()
```

```
Composing complex routes:
- Route transformation: delegates processing to another "inner" route
- Route filtering: only lets requests satisfying a given filter condition pass
- Route chaining: tries a second route if a given first one was rejected
Points 1/2 via Directives
Point 3 via concat method
```

<br>

## Directives



<br>

## Routing DSL Style Guide

```
1. Keep the most static part of a route outermost (e.g. the fixed path segments), end with the HTTP methods:
val prefer = path("item" / "listing") & get
val over = get & path("item" / "listing")

2. Group routes with a pathPrefix where possible, use path for the last bit

3. Create "sub-routes" independently and stitch them together with their prefixes

4.
```

































































