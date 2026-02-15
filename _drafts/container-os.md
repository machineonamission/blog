---
layout: post
title: "Why are modern desktop OSes still so insecure?"
date: 2026-01-09 16:50:00 -0600
---

I've had this idea for ages of a new kind of desktop OS. And, ~~unlike some of my other blog posts~~, basically all the
pieces of this have been made and done before, just not put together like this.

# My issues with modern desktop OSes

I daily drive Windows, and it always makes me weirdly uncomfortable when I'm installing a new app to my computer. 99% of
apps are installed the same way they have been for the past 3 decades, by just dumping files on the system, setting up
services and whatnot. There's so little security. The f\*\*\*ing Meta Quest app auto-starts with your PC, and has no
built-in way to turn it off without messing with Windows services. incredible...

I then pick up my Android phone, and it's an entirely different picture... every app is containerized/sandboxed,
installs are a
single file mediated by the OS, all dangerous permissions are mediated by the OS and explicitly granted by the user,
it's a utopia by comparison!

And it makes me think, why can't desktop OSes work like this?

# People have done it before

There are many existing solutions that do just this. Windows has the Microsoft Store/.msix format for containerized
apps, Linux has Snap (also flatpak kinda?), there's plenty of "atomic" Linux distros, but they have their own issues.

.msix apps either have to go through the microsoft store or roll their own auto-updating solution

Snap is owned by Canonical and the snap store is closed source, and Canonical in general isn't always kind to the
open-source world

flatpak isn't really designed for sandboxing, but it does isolate apps from the underlying filesystem to an extent

# My ideal OS

Disclaimer: i am NOT experienced with OSdev, i just have some ideas i'd like to share. Taking ideas that work well, and
mixing them together in ways I think work well.

## sandboxing

I want app installs to work basically like Android. Installs are contained in a single file, they're managed by the OS,
they get their own little data and cache dir and can't go outside of it.

## security/permissions

Like Android, all dangerous permissions are mediated by the OS and explicitly granted by the user via OS dialogs.

I DO want there to be a "root" permission in the rare case apps do genuinely need access to the underlying OS, and I
don't care if the option to show it is hidden deep in settings, but I want it to be built in.

Also, much like Android (and unlike iOS), these permissions should be very extensive. The more explicit permissions, the
more control the user has, and the less need to rely on more dangerous permissions (like root). Android has tons of
bizarre niche permissions, and my ideal OS would have a similar concept.

## package management

This is where my idea starts to deviate a bit from Android. I do very much like that Android apps can easily be updated,
but if you want true auto updates, you have to go through the play store or use some jank not-as-powerful 3rd party
solution. In my opinion, a full desktop OS needs apps outside its official store to be just as powerful.

This is where I look to Linux package managers like APT. Yes, they do just dump files on the hard drive in the end, but
they, on their own, handle installs, updates, removals, dependencies (will get back to that in a moment), and with full
support for 3rd party sources. THIS is how I think desktop package management should work.

Shared dependencies are wonderful, they simplify things for the dev and reduce duplicated code.

Though APT alone has some problems that I think can be fixed with solutions from other places. If 2 apps have
dependencies that conflict, it just can't install. You have to pick one version of a dependency. Cargo, Rust's package
manager, doesn't have this issue. If dependencies conflict, the package manager just downloads both and provides the
right version to the app that needs it. I believe Windows does something similar with its .dll library manager, letting
apps have different versions of the same library and automatically providing the right version. This is how dependency
management should be done.

Now, combining shared/external dependencies with sandboxed apps isn't as common. Android can't do this much, but Snaps
and .msix mostly can. Bundling everything in the app is secure, sure, but a bit wasteful, no?
Plus, true dependencies open the door for more complex things like apps that depend on entire other apps. WebView no
longer has to be a special system API, it can just be another dependency in the graph. Dependency authors can update and
fix issues as they see fit without the app devs having to update. Imagine Java apps just automatically installing the
JRE, whatever version they need. Same idea for Python, C#, etc.

The main benefit of a true package manager would be auto-updating. It is not trivial to write an app that auto-updates
nowadays without going through a "store". It shouldn't be that way.

## Backwards compatability

The OS is very much a radical change, but if it were to ever be made and try to succeed, it would have to work with some
stuff. Now, I don't know much about the implementation details, but I imagine something like Flatpak where a normal
Linux desktop app can get its own fake virtual filesystem (chroot?), it can call on other apps that the user/package
author allows, get access to normal Linux APIs, but calls to dangerous ones freeze and first ask the user to grant the
permission. a lot like how web apps ask for location permissions, you just have to try to get the location and the
browser will ask the user if it's ok. Weird system APIs can act more like a translation layer than just a fake
filesystem.

## Desktop-class features

Android was never designed as a full desktop OS and this OS would need those missing features. Android's framework for
user-swappable launchers is great, just do that with entire desktop environments. Have apps be able to request
permissions like "open on startup" or "run in the background" or "don't close when minimized", etc. I think iOS's
approach to files actually works well here in some form: by default, apps can request their own user-accessible folder
in the file browser that they can do whatever with and the user can interact like any other folder. Apps can also
request access to specific files/folders, or a one-time popup to ask the user to provide a file. I REALLY do NOT want
apps implementing their own file selection UI because 1. that would require the app access to the entire filesystem
and 2. those are always so ugly and janky and barely functional. Maybe provide an API (i think iOS has this) to embed
the OS file picker into the app, but the app still only sees what the user allows.