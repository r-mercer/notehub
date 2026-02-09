# Rust Notes on Actix WebServer

The Actix route information decoration things are using build in macros that specify a method and a path that the handler should respond to.

Registering manual routes (routes that do not use a routing macro) can be done manually, but that is less of a controller method and more of a function call

“Next, create an App instance and register the request handlers. Use App::service for the handlers using routing macros and App::route for manually routed handlers, declaring the path and method. Finally, the app is started inside an HttpServer which will serve incoming requests using your App as an "application factory”.”
