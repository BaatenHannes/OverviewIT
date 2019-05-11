There are large differences between Web Server Software and the way they handle HttpRequests. This article covers Internet Information Services.

Hosting ASP.NET Core in IIS <https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-2.2>

IIS and Application Pools Explanation <https://stackify.com/iis-web-server/>

### IIS

Web servers handle requests in different ways. Two main ways are to:

* handle all requests on a single thread (with worker threads, e.g. Node.js)
* spawn a new thread for each request (IIS)

#### Application Pools

A single application pool creates at least one worker process (w3wp.exe). This process runs an instance of your application and handles the request send to your web server. (These application pools are visible in IIS Manager). This process runs your application code or returnes a static page.

IIS creates a virtual user for each app pool. These users can also be viewed in your file explorer, as they each have their own folders (music etc.).

App pools recycle by default every 29 hours or when a config file changes. This recycling frees up memory used by runaway processes.

*Advantage of using application pools*
An Application Pool can contain one or more applications and allows us to configure a level of isolation between different Web applications. For example, if you want to isolate all the Web applications running in the same computer, you can do this by creating a separate application pool for every Web application and placing them in their corresponding application pool. Because each application pool runs in its own worker process, *errors in one application pool will not affect the applications running in other application pools*. 

### HttpRequest Lifecycle

Web Applications require a lot of technologies to build our code .When the user interacts with the app causes several HTTP request to run parallel. The http request life cycle is described as below:-

* Web Browser submits the request to the server after creating a socket in local progress.
* The http server waits for the request to come on socket i.e. port-80.
* The web browser translates the yahoo.com to an IP address if it does not knows it.
* If the web browser does not recognize the address it contacts a DNS Server to resolve the name.
* The browser will open a TCP connection to the IP address of yahoo.com and send a HTTP GET request over.
* The request is then submitted to DNS server where IP address is fetched based on domain name.
* Now the request is submitted to Http Server as per HTTP protocol.
* Http server receives the request and shifts the client to another socket.
* So the socket on port-80 is released to receive request from other clients.
* Web Browser & server both are connected to each other.
* The server will process the request, renders the response & breaks the connection.

![HttpLifeCycle](/img/httplifecycle.png)

More information on <https://docs.microsoft.com/en-us/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#http-request-processing-in-iis>

1. When a client browser initiates an HTTP request for a resource on the Web server, HTTP.sys intercepts the request.
1. HTTP.sys contacts WAS to obtain information from the configuration store.
1. WAS requests configuration information from the configuration store, applicationHost.config.
1. The WWW Service receives configuration information, such as application pool and site configuration.
1. The WWW Service uses the configuration information to configure HTTP.sys.
1. WAS starts a worker process for the application pool to which the request was made.
1. The worker process processes the request and returns a response to HTTP.sys.
1. The client receives a response.

![HttpRequest](/img/iishttprequest.png)