# Requests, pages and routing

## Request and pages

A web framework is responsible to provide different resources under different paths\(routes\). When a browser visits a page, let's say. `http://example.com/`. It sends a HTTP request to the server responsible for `example.com` asking for the resource under `/` and specified it's a `GET` request. Which literally translates to the following \(HTTP/1.x is a text based protocol\). You can see how browser specifics which host \(because there may be multiple virtual hosts living on a single server\) and the resource it wants.

```
GET / HTTP/1.1
Host: example.com
```

The `app().registerHandler()` function takes 2 variables, the _path_ and a _handler_. 

* _path_ is the route where the handler would be associated with
* _handler_ is a callback function which generates the desired response

{% code title="routing.cpp" %}
```bash
app().registerHandler("/hello", [](const HttpRequestPtr& req,
                       std::function<void (const HttpResponsePtr &)> &&callback)
{
    auto resp = HttpResponse::newHttpResponse();
    resp->setBody("Hello World");
    callback(resp);
});

app().registerHandler("/answer", [](const HttpRequestPtr& req,
                       std::function<void (const HttpResponsePtr &)> &&callback)
{
    auto resp = HttpResponse::newHttpResponse();
    resp->setBody("42");
    callback(resp);
});
```
{% endcode %}

![Now we have two resources in the same web app.](../.gitbook/assets/image%20%283%29.png)

Pressing F12 in most browsers \(I'm using GNOME's [Epiphany browser](https://wiki.gnome.org/Apps/Web), but it also works on Google Chrome, Mozilla Firefox, Edge, etc..\) brings up the developer menu. Click on "Network" and reload the page with F5. You should see the request made by the browser. Click it and your browser will show you the detail of the request. Most importantly the request made to the server and the corresponding respond.

![](../.gitbook/assets/image%20%284%29.png)

It's easier to make request through the command line. And is especially useful when debugging as browsers won't make anything other than a `GET` request by default. The same request to "/hello" can be made with a curl command.

```text
❯ curl http://127.0.0.1:8080/hello -v
Hello World
```

The `-v` flag tells curl to verbosely print all the messages.

```bash
❯ curl http://127.0.0.1:8080/hello -v
* Uses proxy env variable no_proxy == 'localhost,127.0.0.0/8,::1'
*   Trying 127.0.0.1:8080...
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
> GET /hello HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.75.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< content-length: 11
< content-type: text/html; charset=utf-8
< server: drogon/1.4.1
< date: Wed, 31 Mar 2021 03:07:20 GMT
< 
* Connection #0 to host 127.0.0.1 left intact
Hello World
```

## HTTP Verbs

You may already know what a HTTP verb is. If not, here is a short version: HTTP verbs is a way to specify what the server should do given the same route among other things. There's 9 of them in HTTP/1.x. But in practice, most applications exclusively make use of GET for everything and POST specifically for API requests that needs authentication. So that's what we will stick to in this tutorial.

Drogon allows all verbs by default. Registering a handler exclusively for POST request simply requires passing an extra parameter to `app().registerHandler()`. 

```cpp
app().registerHandler("/foo", [](const HttpRequestPtr& req,
                       std::function<void (const HttpResponsePtr &)> &&callback)
{
    auto resp = HttpResponse::newHttpResponse();
    resp->setBody("This is a POST method!");
    callback(resp);
}, {Post});
```

Now making a POST request from curl can be done adding a `-X` flag then followed by the verb.

```text
❯ curl -X POST http://127.0.0.1:8080/foo
This is a POST method!
```

curl sends a GET request. Therefor Drogon will return an error for the POST exclusive method if curl doesn't have the flag. Notice the **405 Method Not Allowed** message. It indicates that the method exists but the verb isn't right.

```text
❯ curl http://127.0.0.1:8080/foo -v
* Uses proxy env variable no_proxy == 'localhost,127.0.0.0/8,::1'
*   Trying 127.0.0.1:8080...
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
> GET /foo HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.75.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 405 Method Not Allowed
< content-length: 0
< content-type: text/html; charset=utf-8
< server: drogon/1.4.1
< date: Wed, 31 Mar 2021 03:41:59 GMT
< 
* Connection #0 to host 127.0.0.1 left intact
```

The same could be seen if we try to visit the path from a browser, using the URL bar.

![You cannot visit a route that doesn&apos;t support GET from a browser directly.](../.gitbook/assets/image%20%285%29.png)



