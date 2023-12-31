---
title: Never have separate sign-in routes
description: 
published: true
date: 2023-04-21T11:03:52.774Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:03:47.291Z
---

# Never have separate sign-in routes

In a web app, you have two options when guarding your screen for authentication:

1. If the user is not authenticated, redirect to /sign-in route:

<img src="https://miro.medium.com/max/640/1*wZ18Jg4c4hJQ2a3iXHI1xw.webp">

2. If the user is not authenticated, show the sign in form at the attempted page’s URL without redirection and do not have a separate route for that:

<img src="https://miro.medium.com/max/640/1*UkbkM0WZ-jFzPxSfhOoCRQ.webp">

The former approach was used in the early web because pages were immutable back then, and it felt natural to have a separate page URL just because it had a form and not the other content.

To set up such redirects, you make pages without the authentication check and extract that check to some kind of guards that are called at the routing phase. They make sure the target pages are only shown if the user is authenticated.

## Problems of /sign-in route
### 1. A page without semantics
A rule of thumb is to have a page with a URL if it is something people want to bookmark, revisit, and share. Who wants to share the `/sign-in` URL?

### 2. Need to redirect back
After the user was authenticated, they must be redirected to a page they wanted. This means that such a URL must be passed to the sign-in page in a form of `?back=/profile`

This adds a lot of management and special cases with wrong or missing back URL.

And it just looks messy in the address bar.

### 3. An entry in the browser history
If a user had clicked a `/profile` link, was redirected to `/sign-in?…` and then back to `/profile`, these are three entries in the browser history.

If they click ‘back’, they expect to get to a page where they clicked the profile link. Instead they get back to `/sign-in?…` which redirects them again to `/profile` just because they are authenticated.

Users from the early 2000s are trained to double-click the ‘back’ button to escape those vicious sign-in redirects, but it is not a useful habit we want people to develop.

### 4. Still a case with a route viewed anonymously
In modern web apps, you have a stack of pages in the frontend in memory. You may have the following stack:

```
/ → /profile → /profile/edit
```

Now imagine the user got signed out at `/profile/edit` by timeout.

You may redirect them to `/sign-in`, but you still have `/profile` under it in the stack. That page is inactive and does not rebuild, but it still has some state, may have delayed code to execute, and will be shown if the foreground page pops. And anyway widgets must be designed so that rebuild can happen any time arbitrarily often.

You may set up guards that pop all pages that require authentication, but this hurts the user experience. If they sign in again, their page stack will be lost.

So even if you set up some access guards to extract some authentication checks, you cannot get your pages rid of those checks entirely.

## Setting up an in-place sign in form
So you should not have a dedicated sign-in URL. Instead you show the sign-in form at any URL that requires authentication.

The way you set it up depends on your framework.

For backend apps, there is usually some sort of server redirects that do not cause an actual HTTP redirect but only route the request internally to another handler.

For Flutter apps, I normally do the following:

1. Set up a bloc or any other business logic object that is a source of truth for the authentication state in the app.
1. Make some AuthenticatedOrNotWidget with two builders: one for an authenticated case and one for a non-authenticated case. This widget listens to the authentication bloc and rebuilds if the authentication data changes. If the user gets signed out, then non-authenticated builders of each screen in the stack will take over. If the user gets signed back in, the authenticated builders will be called back again. As a bonus, this widget automatically rebuilds if something in the user profile has changed.
1. If the screens are backed with blocs or change notifiers, make them inherit some superclass that is aware of authentication and only calls their business methods if the authentication is alright. This architecture is app-specific.

Here is the widget from one of my projects:

```
class AuthenticatedOrNotWidget extends StatefulWidget {
  final ValueWidgetBuilder<AuthenticationState> authenticatedBuilder;
  final ValueWidgetBuilder<AuthenticationState> notAuthenticatedBuilder;
  final ValueWidgetBuilder<AuthenticationState>? progressBuilder;
  final ValueWidgetBuilder<AuthenticationState>? failedBuilder;

  AuthenticatedOrNotWidget({
    required this.authenticatedBuilder,
    required this.notAuthenticatedBuilder,
    this.progressBuilder,
    this.failedBuilder,
  });

  @override
  State<AuthenticatedOrNotWidget> createState() =>
      _AuthenticatedOrNotWidgetState();
}

class _AuthenticatedOrNotWidgetState extends State<AuthenticatedOrNotWidget> {
  final _authenticationCubit = GetIt.instance.get<AuthenticationBloc>();

  @override
  Widget build(BuildContext context) {
    return StreamBuilder<AuthenticationState>(
      stream: _authenticationCubit.outState,
      builder: (context, snapshot) => _buildWithState(
          context, snapshot.data ?? _authenticationCubit.initialState),
    );
  }

  Widget _buildWithState(BuildContext context, AuthenticationState state) {
    switch (state.status) {
      case AuthenticationStatus.authenticated:
        return _buildAuthenticated(context, state);

      case AuthenticationStatus.notAuthenticated:
        return _buildNotAuthenticated(context, state);

      case AuthenticationStatus.unknown:
        return _buildProgress(context, state);

      case AuthenticationStatus.failed:
        return _buildFailed(context, state);
    }
  }

  Widget _buildAuthenticated(BuildContext context, AuthenticationState state) {
    return widget.authenticatedBuilder(context, state, null);
  }

  Widget _buildNotAuthenticated(BuildContext context, AuthenticationState state) {
    return widget.notAuthenticatedBuilder(context, state, null);
  }

  Widget _buildProgress(BuildContext context, AuthenticationState state) {
    return (widget.progressBuilder ?? widget.notAuthenticatedBuilder)(
      context,
      state,
      null,
    );
  }

  Widget _buildFailed(BuildContext context, AuthenticationState state) {
    return (widget.failedBuilder ?? widget.notAuthenticatedBuilder)(
      context,
      state,
      null,
    );
  }
}
```
https://gist.github.com/alexeyinkin/5f98643d812bb9e36e26077cbb0412b0#file-no_sign_in_route-authenticated_or_not_widget-dart

And here is an even more convenient widget on top of the first one with only the builder for the target page:

```
class SignInIfNotWidget extends StatelessWidget {
  final ValueWidgetBuilder<AuthenticationState> signedInBuilder;

  SignInIfNotWidget({
    required this.signedInBuilder,
  });

  @override
  Widget build(BuildContext context) {
    return AuthenticatedOrNotWidget(
      authenticatedBuilder: signedInBuilder,
      notAuthenticatedBuilder: (_, __, ___) => MySignInWidget(),
      progressBuilder: (_, __, ___) => MyCircularProgressIndicator(),
    );
  }
}
```
https://gist.github.com/alexeyinkin/30e1753332889f0d2ec4d6dbf7ef0627#file-no_sign_in_route-sign_in_if_not_widget-dart

source : https://medium.com/@alexey.inkin/never-have-separate-sign-in-routes-7c9a6dd4dc7c