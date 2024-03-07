---
layout: post
title: "How to validate data"
date: 2024-03-06
categories: [ASP.net, MVC]
tags: [ASP.net, validation]
---

"asp-validation-summary" is used to display a summary of validation errors that have occurred during form submission. 

```html
<form asp-action="Create">
    <div asp-validation-summary="ModelOnly" class="text-danger"></div>
```

There are three kinds of validation, All, ModelOnly and None.

If it's set to All, all validation errors are displayed.
If it's set to ModelOnly, validation errors related to the model are displayed.
If it's set to None, any validation errors are not displayed.


## Common data attributes for validation
```C#
Required
Range(min, max)
Range(type, min, max)
StringLength(length)
RegularExpression(ex)
Compare(other)
Display(Name = "n")
```

```C#
using System.ComponentModel.DataAnnotations;
...
public class Movie
{
    [Required()]
    [StringLength(30)]
    public string Name { get; set; }
 
    [Range(1, 5)]
    public int Rating { get; set; }
}
```

## Custom validation attributes
```C#
using Microsoft.AspNetCore.Mvc.ModelBinding;
...
[HttpPost()]
public IActionResult Index(Customer customer)
{
    string key = nameof(Customer.DOB);
    // check if it's valid for the key value.
    if (ModelState.GetValidationState(key) ==
            ModelValidationState.Valid) {
        // if the key value is valid, check another custom condition.
        if (customer.DOB > DateTime.Today) {
            ModelState.AddModelError(
                key, "Date of birth must not be a future date.");
        }
    }
    if (ModelState.IsValid) {
        // code that adds customer to database
        return RedirectToAction("Welcome");
    }
    else {
        return View(customer);
    }
}
```