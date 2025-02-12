# Development Server Auto-discovery 

Besides dealing with challenges of Apple Watch development, we needed a solution for reaching the development server we are working on.

## The challenge of discovering local development servers in full stack development

When you are developing a full stack Swift application, you want to easily test and debug your application on both the device (iPhone, Apple Watch, iPad, etc...) as well as your development server. If you are using simulator then setting your host server to `localhost` will work but often we need to test on an actual device. 

## Specific issues with Apple Watch development

This is especially true when it comes to developing on for the Apple Watch. There's no easy input control for the developer change the server address.

## Introduction to Sublimation: A Swift package solution

So I ended up creating a Swift Package to enable automatic discovery of your local development server on the fly called Sublimation. It turns your Vapor server from a mysterious gas to a tangible solid server to connect to.

### Overview of Sublimation's purpose and functionality

The purpose is to optimize developer experience and remove as much need to be an IT expert or tinking with the development environment.

### Server and Client components

For the server and client we need a way to communicate that information without the client knowing where the server is initially. 

```mermaid
flowchart TD
%% Nodes for devices with Font Awesome icons
    subgraph Devices
    iPhone("fa:fa-mobile-alt iPhone")
    Watch("fa:fa-square Apple Watch")
    iPad("fa:fa-tablet-alt iPad")
    VisionPro("fa:fa-vr-cardboard Vision Pro")
    end
    
%% Node for Sublimation service with Font Awesome package icon
    Sublimation("fa:fa-box Sublimation")

%% Node for API server with Font Awesome icon
    Server("fa:fa-server API Server")

%% Edge connections
    Devices <--> Sublimation
    Sublimation <--> Server
```

![Diagram on Sublimation Communication](media/Sublimation-2024-08-22-160443.svg)
    
There's two ways to do this - have a consistent location for fetching the address or a way to discover the service on the network.

## Ngrok integration (initial approach)

The initial approach was using `ngrok` to create an a public host name and entering that info in the app's environment variables `insert picture`. This worked but required the developer to update the environment each time _ngrok_ was restarted. If there was a way to save and fetch the new host name consistently for the developer that would reduce one more step.

### Using Ngrok for public exposure of development servers

The missing piece was get the ngrok url created and storing in the cloud somehow via a fairly simple method. 

### How Sublimation automates the Ngrok process


```mermaid
sequenceDiagram
    participant DevServer as Development Server
    participant Sub as Sublimation (Server)
    participant Ngrok as Ngrok (https://ngrok.com)
    participant KVdb as KVdb (https://kvdb.io)
    participant SubClient as Sublimation (Client)
    participant App as iOS/watchOS App
    
    DevServer->>Sub: Start development server
    Sub->>Ngrok: Request public URL
    Ngrok-->>Sub: Provide public URL<br/>(https://abc123.ngrok.io)
    Sub->>KVdb: Store URL with bucket and key<br/>(bucket: "fdjf9012k20cv", key: "dev-server",<br/>url: https://abc123.ngrok.io)
    App->>SubClient: Request server URL<br/>(bucket: "fdjf9012k20cv", key: "dev-server")
    SubClient->>KVdb: Request URL<br/>(bucket: "fdjf9012k20cv", key: "dev-server")
    KVdb-->>SubClient: Provide stored URL<br/>(https://abc123.ngrok.io)
    SubClient-->>App: Return server URL<br/>(https://abc123.ngrok.io)
    App->>Ngrok: Connect to development server<br/>(https://abc123.ngrok.io)
    Ngrok->>DevServer: Forward request to local server
```

![SublimationNgrok Diagram](media/SublimationNgrok-2024-08-22-175342.svg)


What I found was that we can run `ngrok` on startup and access the newly created url via the local API. To store it in the cloud I found a service called kvdb.io which only required a bucket name and key for storing the public url.

#### Cloud setup for meta-server access (using kvdb.io)

If you haven't already setup an account with ngrok and install the command-line tool via homebrew. Next let's setup a key-value storage with kvdb.io which is currently supported. _If you have another service, please create an issue in the repo. Your feedback is helpful._ 

Sign up at kvdb.io and get a bucket name you'll use. You'll be using that for your setup. Essentially there are three components you'll need:

* **ngrok executable path**
    - if you installed via homebrew it's `/opt/homebrew/bin/ngrok` but you can find out using: `which ngrok` after installation
* **bucket name** from your kvdb.io 
* **key** from your kvdb.io 
    - you just need to pick something unique for your server and client to use
    
Save these somewhere in your shared configuration for both your server and client to access, such as an `enum`:

```swift
public enum SublimationConfiguration {
  public static let bucketName = "fdjf9012k20cv"
  public static let key = "my-"
}
```


#### Server Setup

When creating your `Sublimation` object you'll want to use the provided convience initializers ``SublimationTunnel/TunnelSublimatory/init(ngrokPath:bucketName:key:application:isConnectionRefused:ngrokClient:)`` to make it easier for **ngrok** integration with the ``SublimationTunnel/TunnelSublimatory``:

```swift
let tunnelSublimatory = TunnelSublimatory(
  ngrokPath: "/opt/homebrew/bin/ngrok", //
  bucketName: SublimationConfiguration.bucketName, // "fdjf9012k20cv"
  key: SublimationConfiguration.key, // "dev-server"
  application: { myVaporApplication }, // pass your Vapor.Application here
  isConnectionRefused: {$.isConnectionRefused}, // supplied by `SublimationVapor`
  transport: AsyncHTTPClientTransport() // ClientTransport for Vapor
)

let sublimation = Sublimation(sublimatory: tunnelSublimatory)
```

#### Client Setup

For the client, you'll need to import the ``SublimationKVdb`` module and retrieve the url via:

```swift
import SublimationKVdb

let hostURL = try await KVdb.url(withKey: key, atBucket: bucketName) 
```

### Limitations and challenges encountered

- difficult setup with installing ngrok and setting up configuration for each developer
- ngrok maybe already running?

## Bonjour Implementation

The ultimate goal is something which requires no configuration almost 0 configuration. This is where Bonjour comes in.

`what is Bonjour`



### How Sublimation uses Bonjour for local network discovery

```mermaid
sequenceDiagram
  participant Server as Hummingbird/Vapor Server
  participant BonjourSub as BonjourSublimatory
  participant NWListener as NWListener
  participant Network as Local Network
  participant BonjourClient as BonjourClient
  participant App as iOS/watchOS App
  
  Server->>BonjourSub: Start server, provide IP addresses,<br/>hostnames, port, and protocol (http/https)
  BonjourSub->>NWListener: Configure with server information
  NWListener->>Network: Advertise service:<br/>1. Send encoded server data<br/>2. Use Text Record for additional info
  App->>BonjourClient: Request server URL
  BonjourClient->>Network: Search for advertised services
  Network-->>BonjourClient: Return advertised service information
  BonjourClient->>BonjourClient: 1. Receive and decode server data<br/>2. Parse Text Record
  BonjourClient-->>App: Return AsyncStream<URL><br/>or first available URL
  App->>Server: Connect to server using discovered URL
```

When the Hummingbird or Vapor server begins it will tell Sublimation the ip addresses or host names which are available to access the server from (including the port number and whether to use https or http). This is called a `BonjourSublimatory`. The `BonjourSublimatory` then uses `NWListener` to advertise this information both by send the data encoded using Protocol Buffers as well as inside the Text Record advertised.

The iPhone or Apple Watch then uses a `BonjourClient` to fetch either an `AsyncStream` of `URL` or simply get the `first` one available.

#### Setting up your Server

Create a `BindingConfiguration` with:

* a list of host names and ip address
* port number of the server
* whether the server uses https or http

```
let bindingConfiguration = BindingConfiguration(
  host: ["Leo's-Mac.local", "192.168.1.10"],
  port: 8080
  isSecure: false
)
```

Create a `BonjourSublimatory` using that `BindingConfiguration` and include your server's logger. Then attach it to the `Sublimation` object:

```
let bonjour = BonjourSublimatory(
  bindingConfiguration: bindingConfiguration,
  logger: app.logger
)
let sublimation = Sublimation(sublimatory : bonjour)
```

#### Setting up your Client

On the device, create a `BonjourClient` and either get an `AsyncStream` of `URL` objects or just ask for the first one:

```
let client = BonjourClient(logger: app.logger)
let hostURL = await client.first()
```


### Benefits of the Bonjour approach over Ngrok

- no external dependencies
- minimal configuration

### Implementation details and code examples

Lastly `Sublimation` can be used in Server Side Swift either via a Vapor `LifecycleHandler` or Lifecycle `Service`. 

If you are Hummingbird, you can just add it as a service:

```swift
let sublimation = Sublimation(
  bindingConfiguration: .init(hosts: hosts, configuration: configuration.hosting)
)

var app = Application(
  router: router,
  server: .http1WebSocketUpgrade(webSocketRouter: wsRouter),
  configuration: .init(address: .init(setup: configuration.hosting))
)

app.addServices(sublimation)
```

For `Vapor`, you'd add it to the lifecycle of the app:


```swift
let sublimation = Sublimation(
  bindingConfiguration: .init(hosts: hosts, configuration: configuration.hosting)
)

var app : Application

app.lifecycle.use(sublimation)
```

## Impact on gBeat development

### Performance and developer experience improvements

### Seamless testing across iOS and watchOS devices