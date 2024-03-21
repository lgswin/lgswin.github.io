---
layout: post
title: "Implementing Simple Cookie Management in ASP.NET Core with C#"
date: 2024-03-20
categories: [ASP.Net, cookie]
tags: [ASP.Net, C#, cookie]
---

Cookies are a fundamental aspect of web development, allowing web applications to store small pieces of data on the client-side. In ASP.NET Core, managing cookies is straightforward, thanks to the built-in support provided by the framework. In this article, we'll explore how to write and read cookies in ASP.NET Core using C#.

Writing a Cookie:
The WriteCookie method in the provided code snippet demonstrates how to create and set a cookie in ASP.NET Core. Let's break down the implementation:

```csharp
public void WriteCookie(string key, string value, bool isDelete)
{
    CookieOptions options = new CookieOptions();
    if (isDelete)
        options.Expires = DateTime.Now.AddDays(-1);
    else
        options.Expires = DateTime.Now.AddSeconds(10);

    HttpContext.Response.Cookies.Append(key, value, options);
}
```

This method takes three parameters: key, value, and isDelete.
isDelete is a boolean flag indicating whether the cookie should be deleted.
Inside the method, a new CookieOptions object is created to specify additional settings for the cookie.
Depending on the value of isDelete, the Expires property of CookieOptions is set to either expire the cookie immediately or in 10 seconds.
Finally, the cookie is added to the response using HttpContext.Response.Cookies.Append.
Reading a Cookie:
The ReadCookie method retrieves the value of a cookie based on the provided key:

```csharp
public string ReadCookie(string key)
{
    string cookie = HttpContext.Request.Cookies[key];
    return cookie;
}
```

This method takes a key parameter and returns the corresponding cookie value.
It accesses the cookies collection from the incoming HTTP request using HttpContext.Request.Cookies.
Usage Example:
Here's an example of how you can use these methods in a controller action:

```csharp
public IActionResult Index()
{
    // Write a new cookie
    WriteCookie("UserID", "123", false);

    // Read the value of an existing cookie
    string userId = ReadCookie("UserID");

    return View();
}
```

In ASP.NET Core, managing cookies is essential for maintaining state and user authentication in web applications. The provided code demonstrates how to write and read cookies using C#. By leveraging the built-in functionalities of ASP.NET Core, developers can efficiently handle cookies and enhance the user experience of their web applications.