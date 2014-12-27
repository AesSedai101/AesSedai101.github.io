---
layout: post
title:  "Extracting JPEG DCT coefficients"
date:   2014-07-10 23:08:00
categories: c++ jpeg jpg dct libjpeg
---
Extracting the DCT coefficients from a JPEG image seems like a fairly simple task. However, there is almost no examples or documentation online on how to do this - most people just suggest using libjpeg.

So for your convenience: a C++ program making use of libjpeg. This program will ouput the DCT coefficients to standard out (one coefficient per line). Specify the JPEG file in the <i>filename</i> variable.

<script src="https://gist.github.com/AesSedai101/af8634c812921a4d0716.js"></script>