---
layout: post
title:  "Java's Arrays.asList quirk"
date:   2014-04-20 20:40:00
categories: android RuntimeException java
location: github
---
Recently, I was working on an program that used arrays heavily. Since I'm lazy and I needed to sort the array, I converted the array to a list in order to use the built-in functions.

<script src="https://gist.github.com/AesSedai101/e5cac791a4125c11b309.js"></script>

Since the pixels are now conveniently in a list, I can sort them every which way I want.

<script src="https://gist.github.com/AesSedai101/148bbce8d938eb0abc33.js"></script>

The results weren't *quite* what I expected. This is because of a very important line of text in the Java API:
<blockquote>
Returns a fixed-size list **backed by the specified array**. 
</blockquote>

This means that if you sort the list, the array will be sorted as well. Luckily this is easy to fix - just copy the array before you create a list from it:
<script src="https://gist.github.com/AesSedai101/0f2c715172ef4e367fdd.js"></script>
