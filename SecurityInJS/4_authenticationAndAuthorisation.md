## Authentication and authorisation web standards

Authentication and authorisation are necessary components of web apps that allow users to interact  with the system while maintaining the security of the system. Authentication is responsible for verifying a user's credentials, such as their username and password, and being able to derive if the user does in fact have access to the system. Once a user is authenticated, authorisation ensures that the user only has permission to interact with the components of the system that the system admins define. This assures users with malicious intent do not have access to external parts of the system and cannot modify critical data.

Authentication methods come in various form with different levels of security. **Single-factor authentication** only requires one credential (e.g. email or username) against a single password, **two-factor authentication** also requires an extra piece of information - usually as an email, or through an authentication app such as [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en_AU), and **multi-factor authentication** which requires two or more extra uncoupled authorisation credentials. The more unrelated authentication layers used, the greater the security provided against attackers. In most cases, it's recommended to use at least two-factor authentication to keep accounts secure.

These two steps are necessary for managing client credentials and access, and are used most commonly within two open-standard tools: **OpenID** and **OAuth**.



### OpenID Authentication and SSO

If you've ever signed into an online service using your Google or Facebook account, chances are you may have heard of OpenID or OpenID connect. OpenID is a part of the OAuth specification and is responsible for the authentication side of user interactions. OpenID is the protocol that allows you to sign in to a service using an account from another unrelated service, such as using your Facebook account to sign in to Spotify. This is known as a **Single Sign-On** ***(SSO)***, and it determines how you can utilise a single set of credentials among multiple services.

The most significant benefit when using an OpenID authentication protocol is the fact that there is less risk in other services to store or manage your credentials in possibly insecure methods. Since your credentials are only stored on one service, if you sign up to a site that has poor security implementations, there is little chance attackers can compromise your credentials. Of course, the opposite also applies: if your core provider is breached, then it is possible for attackers to gain access across all your OpenID services. For users, however, the benefit of being able to sign in with a single click and not having to remember another set of passwords provides enough confidence to use OpenID authentication whenever possible. 



### OAuth: What it is, How it works

OAuth is an open authorisation standard used to verify users across different services. For example, OAuth is the framework that allows you to use your GitHub account to allow services to interact with your GitHub repositories.

> "OAuth is an open-standard authorisation protocol or framework that describes how unrelated servers and services can safely allow authenticated access to their assets without actually sharing the initial, related, single logon credential. In authentication parlance, this is known as secure, third-party, user-agent, delegated authorisation."
>
> https://www.csoonline.com/article/3216404/what-is-oauth-how-the-open-authorization-framework-works.html

OAuth systems operate similar to basic client-server web-token models, however, the authentication system acts as a middleware between the two; it assures the user is authorised externally through the OAuth provider by verifying the user and sending an access token back through the URL. This access token can then be checked on the server to authorise the user. 

<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/4_oauth.png" alt="oauth-model" style="zoom:80%; max-width: 100%;" /></p>

The core benefit of OAuth is to create a seamless but secure experience when user services interact with each other. You can use a single account to interact with multiple services, and that account can only provide data access to services if you've agreed with them. This gives users granular control over where their data is being used, while also reducing the credentials required to remember.  This concept is known as **scopes**, and they determine what data the service can request from the user. Each API service provides their own set of scopes that abide by OAuth protocols, thus making it easier for developer to enable secure service-to-service communication. For example, the scopes provided by GitHub in their [documentation](https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/) can grant services access to read/write to repos, manage public keys, invite members, manage user data, and more.

The OAuth framework aims to provide a stable and secure platform for developers to easily implement rigid authentication and authorisation processes, and with adoption from big tech companies over the last few years including Apple, Google, Facebook and Twitter, is showing promising results. 

&nbsp;  
&nbsp;  

*Mozamel Anwary ~ March 22, 2020*