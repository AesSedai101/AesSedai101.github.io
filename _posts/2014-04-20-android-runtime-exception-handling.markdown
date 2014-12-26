---
layout: post
title:  "Android runtime exception handling"
date:   2014-04-20 12:00:00
categories: android RuntimeException java
---
If an Android app throws a RuntimeException, the app will close and the user will be informed that the app closed unexpectedly. This is a bad user experience. Luckily, it is possible to catch these exceptions and handle them gracefully.

The first thing you'll need to do is to create your own custom UncaughtExceptionHandler. This specific error handler will pass the error message and the stacktrace to a new activity to handle.

<script src="https://gist.github.com/AesSedai101/ca81c6bdc0b97fce5e36.js"></script>
The next step is to register your exception handler inside your application object.

<script src="https://gist.github.com/AesSedai101/166803608c63d77cd7ad.js"></script>
The last thing to note is that if you want to start an Activity from the error handler, you'll need to make sure that the error activity starts in a **different process** from your main app. This can easily be achieved by adding the process attribute to the ErrorActivity tag in the Android Manifest.

<script src="https://gist.github.com/AesSedai101/af470e1d0c7eb4216744.js"></script>

Simples.