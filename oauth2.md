[Tutorial is from here](https://www.oauth.com/oauth2-servers/getting-ready/)


For Every OAuth2 Application few steps are standard across the providers:
## 1. Creating an Application
The registration process typically involves creating an account on the service’s website, then entering basic information about the application such as the name, website, logo, etc. After registering the application, you’ll be given a client_id (and a client_secret in some cases) that you’ll use when your app interacts with the service.

One of the most important things when creating the application is to register one or more redirect URLs the application will use. The redirect URLs are where the OAuth 2.0 service will return the user to after they have authorized the application. It is critical that these are registered, otherwise it is easy to create malicious applications that can steal user data.

## 2. Redirect URLs and State

OAuth 2.0 APIs will only redirect users to a registered URL, in order to prevent redirection attacks where an authorization code or access token can be intercepted by an attacker.In order to be secure, the redirect URL must be an http.

Most services treat redirect URL validation as an exact match. This means a redirect URL of https://example.com/auth would not match https://example.com/auth?destination=account. It is best practice to avoid using query string parameters in your redirect URL, and have it include just a path.

Some applications may have multiple places they want to start the OAuth process from, such as a login link on a home page as well as a login link when viewing some public item. For these applications, it may be tempting to try to register multiple redirect URLs, or you may think you need to be able to vary the redirect URL per request. Instead, OAuth 2.0 provides a mechanism for this, the **“state”** parameter.

The **“state”** **parameter can be used to encode application state**, but it must also include some amount of random data if you’re not also including PKCE parameters in the request.

The state parameter is a string that is opaque to the OAuth 2.0 service, so whatever state value you pass in during the initial authorization request will be returned after the user authorizes the application. You could for example encode a redirect URL in something like a JWT, and parse this after the user is redirected back to your application so you can take the user back to the appropriate location after they sign in.






