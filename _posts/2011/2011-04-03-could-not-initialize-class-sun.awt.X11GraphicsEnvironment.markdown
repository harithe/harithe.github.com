---
layout: post
title: Could not initialize class sun.awt.X11GraphicsEnvironment
---
today I met an error such as Could not initialize class sun.awt.X11GraphicsEnvironment, it appears that the Java runtime expects some form of X interaction, so just add the following flag to the server invocation:

`-Djava.awt.headless=true`

but seems sometimes, this flag didn't work well, so there is alternate flag:

`unset DISPLAY`

The shell command `unset DISPLAY` is not very well known. It removes any references to remote X11 displays, including ones that have been forwarded over ssh connections.