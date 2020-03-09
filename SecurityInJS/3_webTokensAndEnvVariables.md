## Web tokens and utilising environment variables

When building a webapp with some form of server interaction - whether it's through your own server, a BaaS *(backend-as-a-service)*  implementation, or an external API - it is important to keep your tokens and secrets safe from any users. Leaking these tokens or secrets outside of your access may allow others to gain access to your server or your API source, which can lead to DDOS *(Distributed Denial of Service)* attacks, exhausting your BaaS quota/API calls (which may be costly for a paid service), and possibly giving attackers a gateway to gain access your server. 

**Secrets**, or **tokens**, are keys provided to a client to access an API or backend service. Usually, a client requests access to a server with a username and password. After the server receives this information and can verify you're a user of this service, it generates a token and sends it back to the client. The client can then send this token through HTTP request headers when making requests to the server, and the server can safely send back requested data when verifying that it gave out this token. 

&nbsp;  



<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/3_client-server-img.png" alt="client-server-token-model" style="zoom:30%;" /></p>

### JSON Web Tokens

The most common form of tokens used in the modern web are **JSON Web Tokens *(JWTs)***. A JWT is a secure and compact form of securely sending and receiving data. They are commonly used to authenticate or authorise users through a client or service by securely sending credentials to be verified to a server.

JWTs are sent as encoded strings and decoded into 3 JSON objects: 

1. The **header** which specifies the type of token and the algorithm used to decode data
2. The **payload** which includes the data to be transmitted, usually including user data and the user's permission level
3. A **signature** which is a method that encodes the user and payload data using a secret and the algorithm mentioned in the header; this ensures the data was not tampered with during transmission

The string is sent with 2 dots between each section to specify the encoded header, payload and signature components that should be decoded. For example:

```javascript
/* Encoded format */
// Formatted as: header.payload.signature
xxxxxxx.yyyyyyy.zzzzzzz

/* Decoded format example */
// Header:
{
  "alg": "HS256",
  "typ": "JWT"
}

// Payload:
{
  "sub": "1234567890",
  "name": "Firstname Lastname",
  "admin": true
}

// Signature Verification:
{
  HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
   	secret)
}

```

JWT's small structure makes it a fast tool to send information securely between clients and servers. JSON format is also easy to utilise in almost every modern language in web development, since it can be easily converted into an object (i.e. keys with values). Further details about the structure and implementation of JWTs can be found on the [official JWT intro page](https://jwt.io/introduction/)



### Hiding secrets or tokens 

