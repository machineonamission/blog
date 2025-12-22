---
layout: post
title: "A dream for a better web"
date: 2025-11-17 9:50:00 -0600
---

In this blog article, I outline the issues I’ve had with the web, and a surface-level proposal for a radical rewrite of the web, keeping its strengths and improving its weaknesses.

# The current web

## What it does wrong

In summary, HTML/CSS/JS were designed decades ago with entirely different goals and purposes than the modern web.

### HTML

HTML was designed in the 90s as something more akin to modern .md markup. It has been extended over the years, but still kept many of its core philosophies.  
The modern web is used as a framework for building entire apps with intricate and responsive UI, and the backbones for this are descended from a text markup language.  
HTML lacks many features that many modern web frameworks and other app frameworks add in: named reusable components, reactivity, even the formatting of HTML can be a hurdle to some modern development, specifying properties on one line. Most modern web frameworks replace HTML with something that compiles to it, and this should be noted.

### CSS

Modern CSS is an absolute mess. It inherits technical debt from the 2000s and new features have been grafted on, without addressing the core deficiencies.  
Just ask “how to center a div in CSS” and you begin to see the problems. CSS was never designed for performance, or for complex intricate UIs, and attempting to do this with CSS becomes increasingly annoying and complex. There are so many things I’ve tried to do in CSS that feel so awful and hacky.  
The web is infamously not performant. The complex and layered layout schemes and constant need for hacky workarounds to do basic things does wear on performance.

### JS

The core scripting language of the web, JS, is polarizing. Some love it, some hate it. It’s *fine*, but it has issues.  
Performance: it is an interpreted language at its core, and even JIT compilers can't hope to compare to real compiled code. Even if you could compile JS, it has other strange features like “one” number type.  
Type system: JS’s type system is infamously weird (== vs \===, \[\]==\!\[\]), and it lacks proper type hints (leading to entire projects like TypeScript to graft these onto the language)  
JS has a million quirks that many dislike.  
It has improved over time, gaining features it should have always had like classes, modules, inheritance, etc, but it’s never pleasant to work with bad languages that have had features grafted onto them compared to languages built from the ground up with these in mind.

### WASM

WASM was supposed to be the solution to JS. A platform-independent bytecode that is close to assembly (somewhat like Java’s bytecode) that any language could compile to (or have an interpreter compiled to) while still being safe and performant. However, it has one major weakness: lack of DOM APIs. It is difficult for WASM to interact with HTML directly, having to use JS as a bridge, which makes it annoying to work with, especially as a JS replacement.

## What it does right

### Extensibility

Because of the web’s nature: all code is sent as plaintext and rendered in the browser, it’s extremely extensible. An extension can come along and modify the UI, the scripting, the behavior, or whatever seamlessly. Interpreted languages are also MUCH easier to hook into and modify, since everything is exposed at runtime, and anything can overwrite anything.

### Security

The web has been designed from the ground up for security. You can visit almost any website on the internet and as long as you don't download anything or give any weird permissions, your device can’t get infected.

### Platform independence

Any device with a competent web browser can utilize any website on the internet to its fullest.

### Flexibility

As janky as it is, the modern web is capable of a lot. Modern CSS is a mess, but it’s very capable if you can work around it.

# My proposal

## UI

My proposal’s biggest change would be to no longer use HTML/CSS and instead use a proper UI design language. I’m imagining something like QML, Slint, or a little [React.js](http://React.js) JSX. It should have named reusable components, a simple but powerful markup and layout system, reactivity to state changes, and simpler import systems. Its components should be built from the ground up for ease of use/layout and performance.

Developers would compile their layouts from a human-readable language to an efficient-to-parse bytecode. This could also give the flexibility for others to develop new languages that compile to the same bytecode to add features, like how you can compile other languages to JVM bytecode, though ideally these would get baked into the core language.

Browsers would ship with the runtime to take the layout bytecode and render it, and with many core features of the layout system to prevent reuse and make development simpler. New components could always be added to browsers.

## Scripting

This is the simplest part. Keep WASM, add in DOM APIs (or their equivalent in the new system). Developers use their language of choice (could even be JS\!) and compile it or ship a runtime for that language. I could argue for hours on which language would be best fit, but we don’t need to make that choice when WASM exists. There could also be bundled languages, like JS, but WASM should be the primary focus.

## Extensibility

These new browsers would ship with devtools that can “disassemble” the layout bytecode into something more human-readable, aiding with debugging and extending. Extensions would be able to access and modify the UI tree. Since the UI is taken from bytecode to layout entirely in the browser, layouts can be visually viewed, debugged, and modified in realtime, either by the user in devtools or by extension scripts.

Extension scripts would be able to read and write from WASM’s memory and hook fixed APIs, more reminiscent of mods for phone apps. I’d imagine most extension code would be mostly reading from and writing to the UI anyways, not the website’s internal code.  do acknowledge that this would be as easy as modern extensions, but people have done it before with games and apps, so it’s doable.

# Conclusion

The web is an amazing and massively influential idea, shouldered by decades of technical debt. An amazing solution grafted onto something never designed for it, like building a metropolis on a swamp. I feel a fresh start, with the modern goals in mind, could massively benefit both web devs and web users.
