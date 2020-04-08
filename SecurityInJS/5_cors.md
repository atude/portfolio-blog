## Cross Origin Resource Sharing

When building a website, you often need to request data from an external API that you can process to bring value to your features. Often, for static or small-scale projects, developers choose to make apps with no backend - simply opting in to process everything on the frontend and collating information from various APIs. When requesting some of these sources, you may come across an error similar to this: 

<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/5_cors_error.png" alt="cors-error" style="zoom:30%; max-width: 100%;" /></p>

**Cross Origin Resource Sharing *(CORS)*** is the protocol responsible for allowing applications to request data from another source across different origins/domains. For example, if I had a website hosted on `http://my-website.com` and I had an API endpoint hosted on the same domain that would provide me data, such as `http://my-website.com/users/0`, since both the requesting source and the server source live on the same domain, the server can be confident that the request is coming from a source its intended to server. However, if another website such as `http://not-my-website.com` attempted to access the data from the `users` endpoint, a CORS error would decline the request, since it does not follow the **same-origin policy**.

<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/5_cors_client_server.png" alt="cors-client-server-interaction" style="zoom:30%; max-width: 100%;" /></p>

> <p align="center">How the same-origin CORS policy works. Image from codeacademy.com</p>

Restrictions applied using CORS allows developers building servers to restrict who is able to access resources provided by the server. This can help with reducing server load or prevent external sources from gaining access to private data. Additionally, CORS restrictions can provide limitations on different HTTP requests, including `GET`, `PUT`, `POST`, and others. This can allow the server to be open to all sources for certain requests (e.g. reading data from the server) but deny other requests by, for example, enforcing the same-origin policy for it (e.g. external sites cannot use `POST` requests to write data to the server's database). 

### Using  and customising CORS on JS servers

An application can request a resource from a server using a HTTP request. This can include a `GET` request to receive data, `POST` to upload data, `DELETE` to remove data, and more. In JS applications, you can use the built-in `fetch` function to call a server's endpoint; `fetch` will then attempt to receive the data as a *promise*, and when it's resolved, will either return the data or an error if it failed to process it. This is performed in the frontend of an application; in order for a server to accept or reject certain requests from sources, the server needs to allow certain requests by including headers that can allow specific requests. Some of the important ones include:

- `Access-Control-Allow-Origin`: Which domain is able to access the server

- `Access-Control-Allow-Headers`: Which headers the source has access to

- `Access-Control-Allow-Methods`: Which HTTP methods are allowed on this endpoint

- `Access-Control-Request-Headers`: An indication of what headers the browser may use to tell the server which HTTP request the client will be sending. 

  > The `Access-Control-Request-Headers` option header performs an initial request known as a **preflight** request that executes before the server goes ahead with attempting to process the actual request.

To implement this on your own JS server, simply add the headers into the requests before sending back data. This is an example of opening up the `/users/` endpoint on a server to any domain:

```javascript
app.get('/users/', async (req, res) => {
  // The following header allows ANY app,   
  // from any origin to access this server
  res.header("Access-Control-Allow-Origin", "*");
  res.send(getUserFromDb(req.query));
};
```

This works great on JS backends using common web frameworks such as [Express](https://expressjs.com/) or [Koa](https://koajs.com/), but what if your server hosts multiple endpoints? It would be a hassle to add certain headers before every request manually.  Thankfully, you can add a *middleware* request - or process that executes before any endpoint - to apply the headers across all your requests, using the `app.all` route:

```javascript
// Apply this middleware request to all main HTTP  
// methods across all endpoints on this domain
app.all('/*', (req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Methods", "GET, PUT, POST, DELETE, HEAD");
  // The next() function relays to the next 
  // endpoint that the request satisfies
  next();
});

```

Using this, you can easily route and protect your endpoints to the domains you trust. Enabling CORS also helps prevent overloading your database from malicious requests and mitigating DDOS attacks by blacklisting certain domains.

