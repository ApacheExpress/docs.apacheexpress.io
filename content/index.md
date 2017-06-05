# ApacheExpress

[**ApacheExpress**](http://apacheexpress.io/)
is a way to rapidly write reliable and feature rich web applications
in the [Swift](http://swift.org/) programming language.
Without ever leaving Xcode, or using the Swift Package Manager environment
on Linux.
**ApacheExpress** applications run directly within the 
[Apache WebServer](http://httpd.apache.org/),
as native Apache modules - without any interpreters or proxies.

<center>*Server Side Swift the right way.*</center>

ApacheExpress can be used alongside any other Apache module and hence can
exploit the proven functionality of Apache.
This includes pretty much any kind of authentication mechanism you can think of,
rate limiting / anti-DoS modules, or
mod_ssl to secure the web application as well as mod_h2 to provide
[HTTP/2 support](http2.md). 
The latter are preconfigured when running ApacheExpress
modules and provide full HTTP/2 access out of the box.

## Simple Example

Developing ApacheExpress applications use a Swift version of the mechanisms and
APIs used by the [Express](http://expressjs.com) framework.
Hence its name. Example:

```swift
let app = apache.express(cookieParser(), session())

app.get("/express/cookies") { req, res, _ in
  try res.json(req.cookies)
}

app.get("/express/") { req, res, _ in
  let values = [
    "viewCount"   : req.session["viewCount"] ?? 0,
    "cowOfTheDay" : cows.vaca()
  ]
  try res.render("index", values)
}
```

It grabs the application object representing the module,
configures two builtin middleware objects (`cookieParser`, `session`)
and then registers two [routes](routing.md).
The middleware attached to the `/express/cookies` route simply extracts all
HTTP cookies set in the request and returns them as JSON.
The other sets up a dictionary which is then rendered using a 
[Mustache template](templates.md).


## Database Access

To provide access to a collection of SQL databases, including PostgreSQL and
SQLite3, ApacheExpress uses the Apache [mod_dbd](mod_dbd.md) module.
Apache is used to configure the database connection and Apache is maintaining
the database connection pool.

ApacheExpress then provides the [`mod_dbd`](mod_dbd.md) middleware,
which stacks the powerful [ZeeQL](http://zeeql.io/) database access framework
on top of Apache's module.

#### WORK IN PROGRESS, STAY TUNED

ApacheExpress is still being prepared. Please stay tuned for the release.
If you want to stay up to date, subscribe to the blog
on: [apacheexpress.io/blog/](http://apacheexpress.io/blog/).


