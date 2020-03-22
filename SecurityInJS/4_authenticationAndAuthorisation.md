## Authentication and Authorisation

Authentication and authorisation are necessary components of web apps that allow users to interact  with the system while maintaining the security of the system. Authentication is responsible for verifying a user's credentials, such as their username and password, and being able to derive if the user does in fact have access to the system. Once a user is authenticated, authorisation ensures that the user only has permission to interact with the components of the system that the system admins define. This assures users with malicious intent do not have access to external parts of the system and cannot modify critical data.

Authentication methods come in various form with different levels of security. **Single-factor authentication** only requires one credential (e.g. email or username) against a single password, **two-factor authentication** also requires an extra piece of information - usually as an email, or through an authentication app such as [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en_AU), and **multi-factor authentication** which requires two or more extra uncoupled authorisation credentials. The more unrelated authentication layers used, the greater the security provided against attackers. In most cases, it's recommended to use at least two-factor authentication to keep accounts secure.



### OAuth: What it is, How it works

If you've ever signed into an online service using your Google or Facebook account, chances are you may have heard of OAuth. OAuth is an open authorisation standard used to verify users across different systems. For example, OAuth is the framework that allows you to use your Google account to sign up to social media sites, or GitHub account to allow services to interact with your GitHub repositories.

> "OAuth is an open-standard authorisation protocol or framework that describes how unrelated servers and services can safely allow authenticated access to their assets without actually sharing the initial, related, single logon credential. In authentication parlance, this is known as secure, third-party, user-agent, delegated authorisation."
>
> https://www.csoonline.com/article/3216404/what-is-oauth-how-the-open-authorization-framework-works.html

The core benefit of OAuth is to create a seamless but secure experience when user services interact with each other. You can use a single account to sign into multiple accounts, and that account can only provide data access to services if you've agreed with them. This gives users granular control over where their data is being used, while also reducing the credentials required to remember.  

OAuth systems operate similar to basic client-server web-token models, however, the authentication system acts as a middleware between the two; it assures the user is authorised externally through the OAuth provider by verifying the user and sending an access token back through the URL. This access token can then be checked on the server to authorise the user. 

<p align="center"><img src="https://raw.githubusercontent.com/atude/portfolio-blog/master/_assets/4_oauth.png" alt="oauth-model" style="zoom:30%; max-width: 100%;" /></p>

