# Akka HTTP

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

































































