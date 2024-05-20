---
layout: post
title: Some Minimal API patterns
categories: [Development]
tags: [ASP.Net]
fullview: true
---

Minimal API is a fairly new way to implement Web APIs, introduced in .NET 6.  
I initially thought it was only suitable for small projects, as its name suggests. Recently, I read some articles and watched videos about how to organize Minimal API projects, and I have changed my mind.  
The main reason I believed Minimal API was only for small projects is that most demos are quite small and put all APIs in `Program.cs`, which is obviously not scalable and somewhat messy.  
By using extension methods, it is quite convenient to group APIs into different files, making them much easier to manage.  
A third-party library [Carter](https://github.com/CarterCommunity/Carter) also seems useful in grouping APIs.  
  
Below are some articles and videos I found useful regarding organizing Minimal API applications:  
[Tess Ferrandez's article about organizing ASP.NET Core Minimal APIs](https://www.tessferrandez.com/blog/2023/10/31/organizing-minimal-apis.html)  
[Milan Jovanović's video](https://www.youtube.com/watch?v=GCuVC_qDOV4)  
  
In my previous experience with Spring Boot, I was used to a more MVC style when implementing Web APIs. However, I am glad to see a new way to develop them. I'm not sure if Minimal API is suitable for "large" projects, but it really looks promising to me for many Web API applications.  
