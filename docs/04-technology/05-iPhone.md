# Complementary iPhone App and WatchConnectivity

One of the issues we had with both apps being only on the Apple Watch was the lack of discoverability when it came to the app store. The Apple Watch app store especially in its early days was difficult to work with and communicate with users.

## Developing a companion iPhone app for the AppStore

Much of the code base is shared 

``` 
diagram of code base sharing
```

However the SwiftUI is separated considerably considering how different the devices are. 

## Implementing WatchConnectivity

### Overview of WatchConnectivity framework

WatchConnectivity tries it's best attempt to communicate both messages and the connectivity of the watch and phone. 

``isReachable``
``isWatchAppInstalled`` and ``isCompanionAppInstalled``

``updateApplicationContext``
``sendMessage``

### Combining WatchConnectivity

Combine is used throughout the application. I wanted to take advantage of Combine for dealing with connectivity and communication between the watch and the phone.

https://github.com/brightdigit/SundialKit

I created a library for using Combine with WatchConnectivity:

```
public protocol Messagable {
  /// The unique key or type name to use for decoding.
  static var key: String { get }

  /// Create the object based on the `Dictionary<String, Any>`.
  /// - Parameter parameters: The parameters value.
  init?(from parameters: [String: Any]?)

  /// The parameters of the `Messagable` object.
  /// - Returns: The parameters of the `Messagable` object.
  func parameters() -> [String: Any]
}

public extension Messagable {
  /// Converts the object into a usable `ConnectivityMessage` for
  /// `WatchConnectivity`
  /// - Returns: `ConnectivityMessage` i.e. `[String:Any]` for sending.
  func message() -> ConnectivityMessage {
    [
      MessagableKeys.typeKey: Self.key,
      MessagableKeys.parametersKey: parameters()
    ]
  }
}
```

- startWorkout
- endWorkout
- workout status
- request workout status
