## Making Cookies Secure

You may have come across a popup while using a site, asking you to agree that they will be using cookies, which most of us blindly agree to. A **cookie** is data that's created by a website and stored locally into your web browser so the website can store the state of your action without having to store it on the server. An example of cookies might be a shopping site storing the IDs of products that are in your cart, or a website saving an authentication token to recognise the user and send data back accordingly. Cookies are necessary to track your browsing activity and actions and store them to provide necessary data that the website owners can use to provide features to the user, but also to provide valuable statistic data for the company. This is obviously a privacy concern in itself, but there are also security issues regarding the use of cookies.

### Types of cookies

Cookies aren't all the same - different types of cookies serve different purposes. Different cookies have varied levels of security and access to the user, therefore making them useful to fulfil certain objectives depending on how long it lasts, how it's sent and stored, and who/what has access to it. There are various ***flags*** that can be set on cookies to determine how they behave:

- The **`expires`** flag determines when the cookie is expired. For generic **session cookies**, the cookie is temporary while the user is on the website and deleted once the page is closed. A shopping site may use a cookie like this to track the items that the user has added to the cart if the user doesn't have an account. Storing that data on the server would be pointless and expensive. On the other hand, **persistent cookies** stay stored in the browser for a long, preset time period. This is usually beneficial for keeping tokens stored so users don't have to keep logging back in to websites.
- The **`secure`** flag ensures that the cookie is sent from the server to the client through **HTTPS**. By enforcing encryption when the data is sent, it becomes more difficult for users to interfere with packet's send from the server and read private information.
- The **`http-only`** flag ensures that the cookie is not accessible by the user client-side. This is a vital flag to prevent malicious users from gaining access to sensitive data using JavaScript. The hacker may inspect the client-side to determine the name of the cookie and display it using `console.log(cookieName)` or `alert(cookieName)`. Enabling this flag removes that from potentially occurring, thus also reducing possibility of **XSS** attacks.
- The **`SameSite`** property is a new flag passed into cookies supported by most modern browsers; it determines which domain the cookies must be accessed from, similar to ***CORS***. Setting this property to `Strict` ensures that the cookie is only retrieved from the same domain; this prevents the browser from sending exploits to the website's server (e.g. a hacker may abuse user privileges to send malicious scripts from a third party through the site).

### Setting cookies and keeping them secure

Setting cookies in JavaScript is as simple as setting a variable:

```javascript
document.cookie = "username=Firstname Lastname";
```

This will set the `username` cookie and it will exist until the browser session is closed. To create a more refined cookie, you can add a path, domain (which can help when using the `SameSite` flag)  and set the expiry date:

```javascript
document.cookie = "username=Firstname Lastname; expires=Mon, 10 Jan 2020 12:00:00 UTC; domain=mydomain.com; path=/users;";
```

To enforce security through **HTTPS** and forcing **SameSite** access, append them to the string:

```javascript
document.cookie = "username=Firstname Lastname; domain=mydomain.com; path=/users; secure; samesite=strict;";
```

To access the cookie, simply read `document.cookie`. This will return *all* the cookies within the current domain. For example:

```javascript
document.cookie = "user1=Bob;";
document.cookie = "user2=Smith;";

console.log(document.cookie);

/* Output */
// user1=Bob; user2=Smith;
```

Note that distinct cookies can only be set one by one; the following will not work:

```javascript
/* Note that only the first cookie variable was registered. */
document.cookie = "user1=Bob; user2=Smith;";

console.log(document.cookie);

/* Output */
// user1=Bob;
```

&nbsp;  
&nbsp;  

*Mozamel Anwary ~ April 13, 2020*