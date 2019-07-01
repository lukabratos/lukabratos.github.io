---
layout: post
title: "How to save Swift Enum Types with Associated Values"
date: 2019-06-30 09:38:54 +0000
---

In this blog post we will explore how to create enumerations with associated values, make them conform to the `Codable` protocol by implementing `Decoder` and `Encoder`, and persist them to the file.

## Motivation

In one of the SDKs that I’ve been recently working on we wanted to provide functionality where we could persist all of the network operations to the persistent data store in case our customers device lost network connectivity.
We wanted to be able to retrieve operations later in time from the client in the same order that they’d been enqueued in, and execute every single one of them when the user’s device reconnects to the internet - we wanted to implement a queue. Because we have been using enum with associated values to describe operations in a type-safe way I’ve decided to explore further how could I implement all of the requirements.

## Associated values

Associated values in Swift let us store additional data of any type to enum case values.

Lets say that we have encoded some operations related to the user profile update in an enum:

```swift
enum UserProfileOperations {
    case updateProfilePicture(URL)
    case updateDateOfBirth(Int)
}
```

Constants or variables of `UserProfileOperations` enumeration can either store a value of `updateProfilePicture` with an associated value of type `URL` or a value `updateDateOfBirth` with an associated value of type `Int`.

This example creates a new variable called `profilePictureOperation` and assigns it a value of `UserProfileOperations.updateProfilePicture` with associated value `URL(string: "https://via.placeholder.com/150/1ee8a4")`:

```swift
let profilePictureOperation = UserProfileOperations.updateProfilePicture(URL(string: "https://via.placeholder.com/150/1ee8a4")!)
```

Similar example for `dateOfBirthOperation` variable where we assign UNIX timestamp (associated value of type `Int`) to the value `UserProfileOperations.updateDateOfBirth`:

`let dateOfBirthOperation = UserProfileOperations.updateDateOfBirth(1561825023)`

## Adding Codable conformance

Since Swift 4.1 cannot[^1] automatically synthesize conformance to the `Codable` if we’re using enum with one or more associated values we have to do that manually:

We will start by creating an extension of `UserProfileOperations` enum that conforms to the `Codable` protocol:

```swift
extension UserProfileOperations: Codable {
    ...
}
```

Next, we will define coding keys, which will be used as the authoritative list of properties that must be included when instances of a codable type are encoded or decoded:

```swift
extension UserProfileOperations: Codable {
    enum CodingKeys: CodingKey {
        case profilePictureURL
        case dobTimestamp
    }
}
```

If keys in our JSON payload differ from the ones that we have defined in `CodingKeys` we’ll receive an error.

Because `Codable` is a type alias for the `Encodable` and `Decodable` protocols we need to make sure that our enum conforms to both. Lets start with implementation of the required method for `Decodable`:  [init(from:)](https://developer.apple.com/documentation/swift/decodable/2894081-init):

```swift
extension UserProfileOperations: Codable {
    ...

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        // Decode the value of `profilePictureURL` from the decoder.
        if let profilePictureURL = try? container.decode(URL.self, forKey: .profilePictureURL) {
            self = .updateProfilePicture(profilePictureURL)
            return
        }
        if let dob = try? container.decode(Int.self, forKey: .dobTimestamp) {
            self = .updateDateOfBirth(dob)
            return
        }
        // If the keys used in serialized data format don't match with ones specified
        // in `CodingKeys`, throw an error.
        throw CodingError.message("Error: \(dump(container))")
    }
}
```

Lastly, we need to provide implementation for `Encodable`, which is method [encode(to:)](https://developer.apple.com/documentation/swift/encodable/2893603-encode):

```swift
extension UserProfileOperations: Codable {
    ...

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        switch self {
        case .updateProfilePicture(let url):
            try container.encode(url, forKey: .profilePictureURL)
        case .updateDateOfBirth(let dob):
            try container.encode(dob, forKey: .dobTimestamp)
        }
    }
}
```

## Encoding and Decoding

We can encode `profilePictureOperation` by using [JSONEncoder](https://developer.apple.com/documentation/foundation/jsonencoder):

```swift
let encoder = JSONEncoder()
let profilePictureOperation = UserProfileOperations.updateProfilePicture(URL(string: "https://via.placeholder.com/150/1ee8a4")!)
let data = try! encoder.encode(profilePictureOperation)
let jsonString = String(data: data, encoding: .utf8)!
print(jsonString) // {"profilePictureURL":"https:\/\/via.placeholder.com\/150\/1ee8a4"}
```

And do the reverse, by using [JSONDecoder](https://developer.apple.com/documentation/foundation/jsondecoder):

```swift
let decoder = JSONDecoder()
let profilePictureJSONString = """
{"profilePictureURL":"https://via.placeholder.com/150/1ee8a4"}
"""
let profilePictureOperation = try! decoder.decode(UserProfileOperations.self, from: profilePictureJSONString.data(using: .utf8)!)
```

## Persisting values atomically to the file

Lets create a few instances of our `UserProfileOperations` enum and add them into array:

```swift
let johnProfilePictureURL = UserProfileOperations.updateProfilePicture(URL(string: "https://via.placeholder.com/150/197d29")!)
let johnDateOfBirthOperation = UserProfileOperations.updateDateOfBirth(1561825023)

let kateProfilePictureURL = UserProfileOperations.updateProfilePicture(URL(string: "https://via.placeholder.com/150/c70a4d")!)
let kateDateOfBirthOperation = UserProfileOperations.updateDateOfBirth(733622400)

let timProfilePictureURL = UserProfileOperations.updateProfilePicture(URL(string: "https://via.placeholder.com/150/56acb2")!)
let timDateOfBirthOperation = UserProfileOperations.updateDateOfBirth(586915200)

let operations = [johnProfilePictureURL, johnDateOfBirthOperation, kateProfilePictureURL, kateDateOfBirthOperation, timProfilePictureURL, timDateOfBirthOperation]
```

In the next step we will create a JSONEncoder and use it to encode our operations array into the data object which will contain encoded JSON data.

```swift
let jsonEncoder = JSONEncoder()
let data = jsonEncoder.encode(operations)
```

Finally, we can persist this information into the file:

```swift
let url = URL(fileURLWithPath: "operations")
data.write(to: url, options: .atomic)
```

## Reading values from the file

```swift
let operationsData = try! Data(contentsOf: url)
let jsonDecoder = JSONDecoder()
let operationsArray = try! jsonDecoder.decode([UserProfileOperations].self, from: operationsData)
```

## Conclusion

In this blog post we have looked into associated values and implemented required methods to conform to the `Codable` protocol. We have then encoded and decoded values by using `JSONEncoder` and `JSONDecoder`.
Finally, we have saved the data into the file so it can be retrieved and deserialized back when needed.

I hope you enjoyed this post. Questions? Feedback? Hit me up on [Twitter](https://twitter.com/lukabratos)!

You can download the Swift Playgrounds file [here]({{ site.url }}{{ site.baseurl }}/download/Enums.playground.zip).

[^1]:[Automatic Codable conformance for enums with associated values that themselves conform to Codable](https://forums.swift.org/t/automatic-codable-conformance-for-enums-with-associated-values-that-themselves-conform-to-codable/11499)
