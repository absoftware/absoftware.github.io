---
layout: single
title: "How to make Swift's `[String: Any]` decodable?"
excerpt: "Do you need to decode your JSON to `[String: Any]`? Here is how you can do this."
date: 2020-09-07 23:55:12 +0200
categories: ["iOS"]
tags: ["Swift", "Codable"]
---

Do you need to decode your JSON to `[String: Any]`? Here is how you can do this.

## Requirements

Answer is that you cannot do this using `Any` type. You need specify something more concrete than `Any`.

> Dictionary is automatically decodable if its elements are decodable. Type `Any` is not decodable.

However if you are looking for the answer it means that you actually have some kind of JSON which needs to be handled somehow. The topic is also related to array like `[Any]`.

## Solution

You need to specify finite number of possible values of your `Any` type. For example we expect that there are two different structures like following:

```swift
struct Payload: Codable {
    let id: String
    let eventName: String
    let metadata: [Metadata]
}

struct Metadata: Codable {
    let name: String?
    let price: Double?
    let rating: Int?
}
```

But of course you cannot do

```swift
let decodedData = try JSONDecoder().decode([String: Any].self, from: json.data(using: .utf8)!)
```

because there is used type `Any`.

First of all we need to figure out how JSON for this dictionary possibly looks like. It would be sth like this:

```json
{
    "keyNumber1": {
        "type": "payload",
        "data": {
            "id": "someId1",
            "eventName": "Event Name 1",
            "metadata": []
        }
    },
    "keyNumber2": {
        "type": "metadata",
        "data": {
            "name": "Metadata Name",
            "price": 123.4,
            "rating": 100
        }
    }
}
```

Then it's simple because you need to define `enum` instead of your `Any` type like follows:

```swift
enum DataType: String, Codable {
    case payload
    case metadata
}

enum MyValue: Decodable {
    case payload(_ payload: Payload)
    case metadata(_ metadata: Metadata)

    private enum CodingKeys: String, CodingKey {
        case `type`
        case `data`
    }

    init(from decoder: Decoder) throws {
        let map = try decoder.container(keyedBy: CodingKeys.self)
        let dataType = try map.decode(DataType.self, forKey: .type)
        switch dataType {
        case .payload:
            self = .payload(try map.decode(Payload.self, forKey: .data))
        case .metadata:
            self = .metadata(try map.decode(Metadata.self, forKey: .data))
        }
    }
}
```

And decode JSON using following code:

```swift
let decodedData = try JSONDecoder().decode([String: MyValue].self, from: json.data(using: .utf8)!)
```

Full Playground code:

{% gist 880561fb8ed343304598de2c2de24a29 %}
