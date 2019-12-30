## Tips and tricks

Here are some tips and tricks to work with Puma more efficiently

## Environment variables

When Puma is run as a macOS command line library, you can access environment variables via `ProcessInfo`

```swift
UploadApp {
    $0.authenticate(
        username: ProcessInfo().environment["username"]!,
        password: ProcessInfo().environment["password"]!
    )

    $0.upload(
        ipaPath: Directory.downloads.appendingPathComponent("TestApp.ipa").path
    )
}
```

If you set environment variables in scheme in Xcode, you can choose to have 2 scheme, one shared main scheme and 1 unshared but with environment variables.

## Directory

Puma encourages explicity, so you have to provide explicit paths. Instead of typing path like `/Users/khoa/Downloads`,you can use the `Directory` class which provides nifty properties for popular locations, they are file `URL` so you can append path component further.

```swift
Directory.home
Directory.downloads
Directory.applications
```

## Configure tasks directory

Some tasks like Build, Test, UploadApp uses command line tools like xcodebuild or altool, and they provide convenient configuration methods

```swift
Build {
    $0.configure(workspace: "TestApp", scheme: "TestApp")
}
```

In case you want to customize, you can configure on the command line tool directly

```swift
Build {
    $0.xcodebuild.arguments.append(contentsOf: ["--scheme", "TestApp"])
}
```

## Disable a task

Some times you want to temporarily disable a task, every task in Puma needs to conform to `Task` protocol, and there is required `isEnabled` property where you can toggle off a task

```swift
Build {
    $0.isEnabled = false
}
```

## Change name of a task

Every task has a default name, and this name is used when summarizing, to change the name of a task, assign a different name to `name` property

```swift
Build {
    $0.name = "Build my awesome app"
}
```