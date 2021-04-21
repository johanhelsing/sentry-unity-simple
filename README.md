# Sentry Unity Simple

This is an unofficial Sentry Client for Unity, with a focus on keeping it
simple stupid.

It was forked from the official Sentry Unity Lite in order to add new features.

It sends error logs to a sentry server on exceptions, Debug.LogError and
Debug.LogException.

## Why this project exists

This project was forked from the official stable client, [Sentry Unity
Lite](https://github.com/getsentry/sentry-unity-lite), which is no longer
accepting pull-requests for new features, despite lacking the following:

- Distinction between Debug.LogWarning and Debug.Log
- Toggle sending logs in editor
- Adding extra tags
- It also defaults to sending PII data

There is [a new official client](https://github.com/getsentry/sentry-unity) in
development which will fix these issues, however I personally don't like it
because:

- The installed package doesn't contain the source code, but instead uses
  .dll-s built in a separate step.
- Building the package requires separate tools
- It has a *lot* of features, like getting C++ stack traces on Android,
  performance monitoring, release health. That may be nice, but I just want
  error reporting with simple but useful features.

In short, I wanted a small project that doesn't require a complex build
procedure and is easy to reason about and contribute to, but Sentry Unity Lite
didn't have the features I needed, and is no longer accepting PRs.

If you are used to other Sentry SDKs, you might find that the API here is
smaller. This is by design, and this SDK is lightweight and compile with your
game. It supports any platform that you can target with Unity.

### Installation

#### Through the package manager

![Install git package screenshot](./Documentation~/install-git-package.png)

Open the package manager, click the + icon, and add git url.

```
https://github.com/johanhelsing/sentry-unity-simple.git#1.0.4
```

#### Through unitypackage

The [Releases page](https://github.com/getsentry/sentry-unity-lite/releases) include a `.unitypackage` which you can simply drag and drop into your project.

### Usage

In order to make Sentry work, you need to add `SentrySdk` component to any
`GameObject` that is in the first loaded scene of the game.

You can also add it programatically. There can only be one `SentrySdk`
in your whole project. To add it programatically do:

```C#
var sentry = gameObject.AddComponent<SentrySdk>();
sentry.Dsn = "__YOUR_DSN__"; // get it on sentry.io when you create a project, or on project settings.
```

The SDK needs to know which project within Sentry your errors should go to. That's defined via the DSN.
DSN is the only obligatory parameter on `SentrySdk` object.

This is enough to capture automatic traceback events from the game. They will
be sent to your DSN and you can find them at [sentry.io](sentry.io)

`SentrySdk` is the main component that you have to use in your own project.

### Example

The package includes a Demo scene. `SentryTest` is a component that handles
button presses to crash or fail assert.

### API

The basic API is automatic collection of test failures, so it should mostly
run headless. There are two important APIs that are worth considering.

* collecting breadcrumbs

  ```C#
  SentrySdk.AddBreadcrumb(string)
  ```

  will collect a breadcrumb.

* sending messages

  ```C#
  SentrySdk.CaptureMessage(string)
  ```

  would send a message to Sentry.

### Unity version

The lowest required version is Unity 2018.4. Previous versions might work but
were not tested and will not be supported.
