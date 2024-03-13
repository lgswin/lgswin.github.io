---
layout: post
title: "Json deserialization"
date: 2024-03-13
categories: [C#, Json]
tags: [C#, Json, deserialization, interview]
---


## Find all unique studentId from the json file.

- data.json

```csharp
{
  "uploadDate": 1710335183,
  "data": [
    { "program": "cooking", "timestamp": 1584104801, "studentId": "9a7ed8e9-f3b1-4c6c-a4c7-792bafe332ed" },
    { "program": "hunting", "timestamp": 1584104901, "studentId": "9a7ed8e9-f3b1-4c6c-a4c7-792bafe332ed" },
    { "program": "cooking", "timestamp": 1584105801, "studentId": "bb06dbcd-b6b3-4d38-b2ed-a9c814b0e853" },
    { "program": "cooking", "timestamp": 1584105801, "studentId": "24bbc3e0-3f7a-4f32-91ff-eaf63ceedb6e" },
    { "program": "hunting", "timestamp": 1584108801, "studentId": "24bbc3e0-3f7a-4f32-91ff-eaf63ceedb6e" },
    { "program": "fishing", "timestamp": 1584108801, "studentId": "24bbc3e0-3f7a-4f32-91ff-eaf63ceedb6e" },
    { "program": "cooking", "timestamp": 1584108801, "studentId": "cd2f19f9-76ec-44b7-b08a-540d843b2048" },
    { "program": "hunting", "timestamp": 1584110801, "studentId": "cd2f19f9-76ec-44b7-b08a-540d843b2048" },
    { "program": "cooking", "timestamp": 1584110801, "studentId": "9c39effc-8b1e-4ef8-a9db-5bf424873ad5" },
    { "program": "cooking", "timestamp": 1584110901, "studentId": "91fc2783-9310-46ed-9903-ee7fb43f4bcd" },
    { "program": "hunting", "timestamp": 1584110901, "studentId": "f388820a-6921-45df-b62d-b4d74ca0a131" },
    { "program": "hunting", "timestamp": 1584111501, "studentId": "91fc2783-9310-46ed-9903-ee7fb43f4bcd" },
    { "program": "fishing", "timestamp": 1584111501, "studentId": "f388820a-6921-45df-b62d-b4d74ca0a131" },
    { "program": "fishing", "timestamp": 1584111801, "studentId": "91fc2783-9310-46ed-9903-ee7fb43f4bcd" },
    { "program": null, "timestamp": 1584111801, "studentId": "f388820a-6921-45df-b62d-b4d74ca0a131" },
    { "program": "fishing", "timestamp": 1584112801, "studentId": "da7e5e46-a722-4a49-8d74-af8b5bc9469e" },
    { "program": "pottery", "timestamp": 1584112801, "studentId": "91fc2783-9310-46ed-9903-ee7fb43f4bcd" },
    { "program": "pottery", "timestamp": 1584114801, "studentId": "f388820a-6921-45df-b62d-b4d74ca0a131" },
    { "program": "underwater-basket-weaving", "timestamp": 1584114801, "studentId": "da7e5e46-a722-4a49-8d74-af8b5bc9469e" },
    { "program": "pottery", "timestamp": 1584115801, "studentId": "cd2f19f9-76ec-44b7-b08a-540d843b2048" }
]
}
```

- Main.cs

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Newtonsoft.Json;

public class MainClass
{
    static void Main(string[] args)
    {
        string jsonString = File.ReadAllText("src/data.json");

        var jsonObject = JsonConvert.DeserializeObject<dynamic>(jsonString);

        HashSet<string> uniqueStudentIds = new HashSet<string>();

        foreach (var entry in jsonObject.data)
        {
            if (entry.studentId != null)
            {
                uniqueStudentIds.Add(entry.studentId.ToString());
            }
        }

        foreach (var studentId in uniqueStudentIds)
        {
            Console.WriteLine(studentId);
        }
    }
}

```