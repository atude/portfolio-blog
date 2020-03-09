## Secrets and utilising environment variables

When building a webapp with some form of server interaction - whether it's through your own server, a BaaS *(backend-as-a-service)*  implementation, or an external API - it is important to keep your tokens and secrets safe from any users. Leaking these tokens or secrets outside of your access may allow others to gain access to your server or your API source, which can lead to DDOS *(Distributed Denial of Service)* attacks, exhausting your BaaS quota/API calls (which may be costly for a paid service), and possibly giving attackers a gateway to gain access your server. 

**Secrets**, or **tokens**, are keys provided to a client to access an API or backend service. Usually a client service requests access to a server with a username and password. After the server receives this information and can verify you're a user of this service, it generates a token and sends it back to the client. The client can then send this token through HTTP request headers when making requests to the server, and the server can safely send back requested data when verifying that it gave out this token. 





<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/3_client-server-img.png" alt="client-server-token-model" style="zoom:30%;" /></p>