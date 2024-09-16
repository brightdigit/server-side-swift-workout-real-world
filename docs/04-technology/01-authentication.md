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

  internal func saveToSimulators(
    _ token: String,
    toRelativePath relativePath: String,
    appBundleIdentifier: String
  ) async throws {
    let containerPaths = try await self.fetchContainerPaths(
      appBundleIdentifier: appBundleIdentifier,
      type: .data
    )

    let filePaths = containerPaths.map { $0.appending("/" + relativePath) }

    guard !filePaths.isEmpty else {
      throw MissingSimulatorError(
        appBundleIdentifier: appBundleIdentifier,
        type: .data
      )
    }

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

  internal func fetchContainerPaths(
    appBundleIdentifier: String,
    type: ContainerID,
    deviceState: DeviceState = .booted
  ) async throws -> [Path] {
    try await withThrowingTaskGroup(of: Path?.self) { taskGroup in
      let list = try await self.run(List())
      let devices = list.devices.values
        .flatMap { $0 }
        .filter { $0.state == deviceState }
      for device in devices {
        taskGroup.addTask {
          do {
            return try await self.run(
              GetAppContainer(
                appBundleIdentifier: appBundleIdentifier,
                container: type,
                simulator: .id(device.udid)
              )
            )
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

```swift
extension SimCtl {
  func fetchContainerPaths(
    appBundleIdentifier: String,
    type: ContainerID,
    deviceState: DeviceState = .booted
  ) async throws -> [Path] {
    try await withThrowingTaskGroup(of: Path?.self) { taskGroup in
      // run `xcrun simctl list devices`
      let list = try await self.run(List())
      // filter the devices which are `deviceState` i.e. booted
      let devices = list.devices.values
        .flatMap { $0 }
        .filter { $0.state == deviceState }
      for device in devices {
        taskGroup.addTask {
          do {
            // run
            //  `xcrun simctl get_app_container {.id(device.udid)} {appBundleIdentifier} {type}`
            // example:
            //  `xcrun simctl get_app_container E294F724-10D0-422E-894C-745791166D86 com.bpmsync.GBeat.watchkitapp data`
            let appContainerSubcommand = GetAppContainer(
              appBundleIdentifier: appBundleIdentifier,
              container: type,
              simulator: .id(device.udid)
            )
            // ...
            return try await self.run(appContainerSubcommand)
          } catch GetAppContainer.Error.missingData {
            return nil
          } catch {
            return nil
          }
        }
      }

      return try await taskGroup.reduce(into: [Path]()) { paths, path in
        if let path {
          Self.logger.debug("Discovered Simulator at: \(path)")
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
