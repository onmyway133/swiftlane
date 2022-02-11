# Swiftlane

Swiftlane contains a set of build utilities to speed up iOS and macOS development and deployment

There's no additional configuration file, your Swift script file is the source of truth. With auto 
completion and type safety, you are ensured to do the right things in Puma.

- Swiftlane and its dependencies are written in pure Swift, making it easy to read and contribute.
- Use latest Swift features like async/await to enable declarative syntax
- Type-safe. All required and optional arguments are clear.
- No configuration file. Your Swift script is your definition.
- Simple wrapper around existing tools like xcodebuild, instruments and agvtool
- Reuse awesome Swift scripting dependencies from Swift community

## How to use

Swiftlane is intended to be used as a Swift Package. Simply import it into your Swift script file

```swift
import Swiftlane

func deployMyAwesomeApp() async throws {
    var workflow = Workflow()
    workflow.directory = Settings.default.fs.homeDirectory.appendingPathComponent("Projects")
    
    let build = Build()
    build.project("MyAwesomeApp")
    build.scheme("Production")
    build.configuration(.release)
    build.workflow = workflow
    try await build.run()
    
    let archive = Archive()
    archive.workflow = workflow
    // configure action
    try await archive.run()
    
    let testflight = Testflight()
    // configure action
    try await testflight.run() 
    
    let slack = Slack()
    slack.send(
        message: Slack.Message(text: "Deploy complete")
    ) 
}
```

## Actions

### iOS
- [x] Build: build project
- [x] Test: test project
- [ ] Archive: archive project
- [x] SetBuildNumber: set build number
- [x] SetVersion: set version
- [ ] TestFlight: deploy to testFlight
- [ ] AppCenter: deploy to AppCenter
- [ ] FetchProvisioningProfiles: fetch provisioning profile
- [ ] GenerateIcon: generate app icon set
- [ ] Screenshot: take screenshot
- [ ] Frame: frame screenshot

### macOS
- [ ] Notarize: notarize project
- [ ] MakeDMG: package as DMG
- [ ] Sparkle: update Spackle Appcast file

### Standard
- [x] Slack: send message to a Slack channel
- [x] RunScript: run arbitrary script
- [x] PrintWorkingDirectory: print current working directory
- [ ] UploadS3: upload to S3
- [ ] Download: download files

### File
- [x] MoveFile: move file
- [x] CopyFile: copy file

## Settings

Configurations via `Settings.default`

- Console: log to console
- FileSystem: interact with file system
- Environment: read environment values
- CommandLine: run command line tools

## License
Swiftlane is released under the MIT license. See LICENSE for details.


