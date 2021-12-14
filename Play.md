# Play

<br>

The backbone of a Play web application is made up of Actions, Controllers, and routes:

- Actions are functions from Requests to Results
- Controllers are collections of action-producing methods
- Routes map incoming Requests to Action-producing method calls on our Controllers

We typically place controllers in a Controllers package in the app/controllers folder.
Routes go in the conf/routes file (no filename extension).

<br>

## Routes

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























































