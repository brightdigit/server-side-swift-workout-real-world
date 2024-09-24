# Authentication with no UI

- It's difficult to enter information on the Apple Watch - let alone username and password
- How can we avoid the user having to enter anything
- Heartwitch solution was to link their account via iCloud
- gBeat used Sign in with Apple 

## Implementation across iOS and watchOS

- gBeat uses Sign in with Apple for the user to authenticate via iPhone and Apple Watch

## Challenge: Sign in with Apple not available on Apple Watch simulator

- Developing for the Apple Watch can be a challenge `how is Apple Watch a challenge`
- Using the Simulator makes it even easier
- Simulator can fake HealthKit stats be not Sign In With Apple `what happens`
- How can we fake **sign in with apple**
	 
### Impact on development and testing process 

`link to simulator services and code snippet`

```swift
extension SimCtl {
  struct MissingSimulatorError: Error {
    let appBundleIdentifier: String
    let type: ContainerID
  }

  /// Saves the token to all booted simulator devices in the container directory.
  /// - Parameters:
  ///   - token: Authentication Token
  ///   - relativePath: relative path inside the container.
  ///   - appBundleIdentifier: Application Bundle Identifier.
  ///   - type: Container Type
  ///   - deviceState: Filter Devices by Device State
  internal func saveToSimulators(
    _ token: String,
    toRelativePath relativePath: String,
    appBundleIdentifier: String,
    type: ContainerID = .data,
    deviceState: DeviceState? = .booted
  ) async throws {
    // get container paths
    let containerPaths = try await self.fetchContainerPaths(
      appBundleIdentifier: appBundleIdentifier,
      type: type
    )

    // define file paths
    let filePaths = containerPaths.map { $0.appending("/" + relativePath) }

    // throw error is there's simulator running
    guard !filePaths.isEmpty else {
      throw MissingSimulatorError(
        appBundleIdentifier: appBundleIdentifier,
        type: type
      )
    }

    // write the token to each simulator's path
    try await withThrowingTaskGroup(of: Void.self) { taskGroup in
      for filePath in filePaths {
        taskGroup.addTask {
          try token.write(
            to: URL(fileURLWithPath: filePath),
            atomically: true,
            encoding: .utf8
          )
        }
      }

      return try await taskGroup.reduce(()) { _, _ in }
    }
  }

  /// Fetches container paths for a specific application.
  /// - Parameters:
  ///   - appBundleIdentifier: Application Bundle Identifier
  ///   - type: Container Type
  ///   - deviceState: Filter Devices by Device State
  /// - Returns: A list of paths to all containers based on the application identifier.
  internal func fetchContainerPaths(
    appBundleIdentifier: String,
    type: ContainerID,
    deviceState: DeviceState? = .booted
  ) async throws -> [Path] {
    try await withThrowingTaskGroup(of: Path?.self) { taskGroup in
      // run `xcrun simctl list devices`
      let list = try await self.run(List())
      // filter the devices which match the `deviceState`
      let devices: [Device]
      let listDevices = list.devices.values.flatMap { $0 }
      if let deviceState {
        devices =
          listDevices
          .filter { $0.state == deviceState }
      } else {
        devices = listDevices
      }
      for device in devices {
        // for each device run
        //  `xcrun simctl get_app_container {device.udid} {appBundleIdentifier} {type}`
        // example:
        //  `xcrun simctl get_app_container E294F724-10D0-422E-894C-745791166D86 com.bpmsync.GBeat.watchkitapp data`
        let subcommand = GetAppContainer(
          appBundleIdentifier: appBundleIdentifier,
          container: type,
          simulator: .id(device.udid)
        )
        taskGroup.addTask {
          do {
            return try await self.run(subcommand)
          } catch GetAppContainer.Error.missingData {
            return nil
          }
        }
      }

      return try await taskGroup.reduce(into: [Path]()) { paths, path in
        if let path {
          paths.append(path)
        }
      }
    }
  }
}

```

- On development server save the token as a file on the Mac and copy it to the simulator directly
- Login via web site
- Simulator then watches for the file and _logs in_

### Workarounds and solutions 
