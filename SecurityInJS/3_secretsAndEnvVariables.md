## Web tokens and utilising environment variables

When building a webapp with some form of server interaction - whether it's through your own server, a BaaS *(backend-as-a-service)*  implementation, or an external API - it is important to keep your tokens and secrets safe from any users. Leaking these tokens or secrets outside of your access may allow others to gain access to your server or your API source, which can lead to DDOS *(Distributed Denial of Service)* attacks, exhausting your BaaS quota/API calls (which may be costly for a paid service), and possibly giving attackers a gateway to gain access your server. 

**Secrets** are important information often used to communicate between different services securely; **tokens** are secret keys provided to a client to access an API or backend service. Usually, a client requests access to a server with a username and password. After the server receives this information and can verify you're a user of this service, it generates a token and sends it back to the client. The client can then send this token through HTTP request headers when making requests to the server, and the server can safely send back requested data when verifying that it gave out this token. 

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

When integrating secrets or web tokens in the client-side of a webapp, the tokens must be kept secret to outside users, and only the service hosting the client should have access to it. But, if the token was copied directly into the code, anyone can inspect the codebase and fish for secure tokens. To keep this data away from users but accessible to the hosting client, JS application can utilise **environment variables**. 

Environment variables allows developers to specify constants that can be used inside an app only within a certain environments. It is usually used to define ports to connect to (e.g. connect to `localhost` on local machine and an online server in a hosted environment) and to define API keys and tokens to establish connections with them. It generally requires some configuration (i.e. using the `dotenv` package in JS apps). After this, secret tokens can be defined in a `.env` file, and accessed globally within that environment. A `.env` file is usually in a form like so:

```
NODE_ENV=local
PORT=3000
API_KEY=*************************
```

Although useful, environment variables aren't perfect, and there a few steps required to ensure they are kept safe among yourself and your team. Most methods involve keeping the keys secret and only where they need to be:

1. **Include a `.gitignore `file and add the `.env` file to it. ** This ensures that secret keys aren't uploaded into online source code repositories, which would be the last place you'd want them to be.
2. **Create a `.env.example`and clearly document how to use it.** Less experienced developers may not understand the concepts of keys and tokens, and may end up leaking it unintentionally. Making it clear to your team by using an example file to go from, and by specifying details in a `README` can help reduce the chance of a leak through miscommunication.
3. **Use git commit/push hooks to prevent accidentally uploading keys to source control tools.** Commit hooks such as `husky` allows you to scan your project and check if keys are exposed in your codebase; these hooks will interfere during the commit process and stop these keys from being pushed online.
4. **If absolutely necessary, use tools to filter and overwrite your git history.** In worst-case scenarios, a key may be accidentally leaked to online source control repos. In a large scale project, it's best to immediately reset the key and assume someone may have gained access to it. However, in small-scale or personal projects, you can try overwrite your git history and remove the exposed keys entirely *(highly unrecommended, however)*. Tools such as the [BFG Repo Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) can easily automate this, but note that it will change your commit history considerably. You can find other methods to remove sensitive data on [Github's help page](https://help.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository).