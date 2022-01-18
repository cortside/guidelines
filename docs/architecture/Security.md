# Security

## Use HTTPS

Always use HTTPS.  This includes APIs exposed for external consumption as well as between services inside the private network.

## Use OpenID Connect

OpenID Connect is an open standard for federated identity, built upon OAuth 2.0.  Single Sign On (SSO) is a subset of federated identity management and is the means of linking a person's electronic identity and attributes, stored across multiple distinct identity management systems.

OpenID Connect, using an implementation like [IdentityServer](https://duendesoftware.com/products/identityserver), allows authenticating a user or client in exchange for an access token that can be used to access an API.

OpenId Connect addresses the needs of authentication.

## Authentication vs Authorization

Authentication is the process of verifying who someone is, whereas authorization is the process of verifying what specific applications, files, and data a user has access to.  Authorization and authentication work together.

## Authorization

Authorization in system security is the process of giving the user permission to access a specific resource or function. This term is often used interchangeably with access control or client privilege.

Giving someone permission to download a particular file on a server or providing individual users with administrative access to an application are good examples of authorization.

In secure environments, authorization must always follow authentication. Users should first prove that their identities are genuine before an organization’s administrators grant them access to the requested resources.

## Client Credentials Flow/Grant

Client Credentials flow allows a client to request an Access Token using its Client ID and Client Secret. Both are kept securely on the Client side and registered in an Authorization Server.  This flow is not appropriate for browser based applications nor mobile applications where a client secret can't reasonably be secured. 

## Implicit Flow

Implicit flow is a browser only flow that does not use a client secret, since it can't be secured.

## Token Exchange

Similar to client credentials, but in addition to client id and secret, a token is exchanged for a new token.  Token exchange would be used for delegation or impersonation.

## Beyond Implicit Flow

Implicit flow is now no longer a recommended way of securing a public application.  See writeup [here](https://brockallen.com/2019/01/03/the-state-of-the-implicit-flow-in-oauth2/).
