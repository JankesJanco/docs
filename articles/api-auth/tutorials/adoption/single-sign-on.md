---
title: OIDC Single sign-on
---

# Single sign-on

<%= include('./_about.md') %>

Single sign-on (SSO) occurs when a user logs in to one client and is then signed in to other clients automatically.
In the context of the OIDC-conformant authentication pipeline, SSO must happen at the authorization server (i.e. Auth0) and not client applications.
This means that for SSO to happen, users must be redirected to an Auth0-hosted login page.

We are planning on providing support for SSO from client applications in future releases.

## How SSO works

At a general level, this is what happens when performing SSO:

1. If the user is not logged in locally, redirect them to Auth0 for authentication. This is done using the [authorization code](/api-auth/grant/authorization-code) or [implicit](/api-auth/grant/implicit) grants, depending on the type of application.
2. If the user was logged in through SSO, Auth0 will immediately authenticate them without needing to re-enter credentials.

An application that does not use SSO might decide to show a self-hosted login page to unauthenticated users instead of redirecting to Auth0 for authentication.

## Determining if users are logged in via SSO

An application can easily determine if a user is logged in locally by checking the validity of a local ID token or session cookie.
However, in some cases your application might need to determine if the user has a valid SSO session at Auth0.
In the legacy authentication pipeline, this could be achieved by using the `/ssodata` endpoint, which would return information about the user's SSO session.
OIDC-conformant clients must use [silent authentication](/api-auth/tutorials/silent-authentication), which either re-authenticates a user if they are already logged in, or returns an error if they need to authenticate.

## Authentication flows without SSO

SSO sessions are managed by Auth0 setting a cookie on your Auth0 domain.
Since cross-origin requests cannot set cookies, this means that SSO sessions must be established by redirecting users to your Auth0 login page (`/authorize`).

The following flows are redirect-based and are capable of SSO:

* [Authorization code grant](/api-auth/grant/authorization-code)
* [Implicit grant](/api-auth/grant/implicit)

The following flows are request-based and are currently not capable of SSO:

* [Password credentials and realm grants](/api-auth/grant/password)

## Custom domains

The Auth0-hosted login page is currently hosted at an Auth0 domain, so any SSO will be performed at an Auth0 domain instead of your organization's domain.
This is only an aesthetic limitation and does not impact the security or functionality of SSO logins in any way.

We plan on adding support for custom domains on login pages in future releases via CNAMEs.
