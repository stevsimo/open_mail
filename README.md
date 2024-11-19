# Open Mail

![Flutter 3.24.4](https://img.shields.io/badge/Flutter-3.24.4-blue)
[![pub package](https://img.shields.io/pub/v/open_mail.svg?label=open_mail&color=blue)](https://pub.dev/packages/open_mail)

### Open Mail is a Flutter package designed to simplify the process of querying installed email apps on a device and allowing users to open the email app of their choice.

This package is especially useful for developers who want to provide users with more control over which email app to use, unlike url_launcher, which typically opens only the default email app.

If you just want to compose an email or open any app with a `mailto:` link, you are looking for [url_launcher](https://pub.dev/packages/url_launcher).

This Flutter package allows users to open their installed mail app effortlessly. It draws inspiration from similar packages, particularly [open_mail_app](https://pub.dev/packages/open_mail_app), and builds upon that foundation to provide an improved and streamlined experience.

## Why Use Open Mail?

- `url_launcher` allows you to open `mailto:` links but does not let you:
  - Choose a specific email app.
  - Query the list of available email apps.
- On iOS, `url_launcher` will always open the default **Mail App**, even if the user prefers another app like Gmail or Outlook.
- With **Open Mail**, you can:
  - Identify all installed email apps.
  - Allow users to select their preferred email app (if multiple options are available).

## Setup

### Android

No additional configuration is required for Android. The package works out of the box.

### iOS Configuration

For iOS, you need to list the URL schemes of the email apps you want to query in your `Info.plist` file. Add the following code:

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>googlegmail</string>
    <string>x-dispatch</string>
    <string>readdle-spark</string>
    <string>airmail</string>
    <string>ms-outlook</string>
    <string>ymail</string>
    <string>fastmail</string>
    <string>superhuman</string>
    <string>protonmail</string>
</array>
```

Feel free to file an issue on GitHub for adding more popular email apps you would like to see supported. These apps must be added to both your app’s `Info.plist` and the source code of this library.
Please file issues to add popular email apps you would like to see on iOS. They need to be added to both your app's `Info.plist` and in the source of this library.

## Installation

Add the following to your `pubspec.yaml`:

```
dependencies:
  open_mail: latest_version
```

Then, run the following command:

```
flutter pub get
```

## Usage

### Open Mail App with Picker (if multiple apps are available)

Below is a full example of how to use the package:

```dart
import 'package:flutter/material.dart';
import 'package:open_mail/open_mail.dart';

void main() {
  runApp(MaterialApp(home: MyApp()));
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Open Mail App Example'),
      ),
      body: Center(
        child: ElevatedButton(
          child: Text("Open Mail App"),
          onPressed: () async {
            // Attempt to open a mail app
            var result = await OpenMail.openMailApp();

            // If no mail apps are found, show an error dialog
            if (!result.didOpen && !result.canOpen) {
              showNoMailAppsDialog(context);
            }
            // If multiple mail apps are found (iOS), show a picker dialog
            else if (!result.didOpen && result.canOpen) {
              showDialog(
                context: context,
                builder: (_) {
                  return MailAppPickerDialog(
                    mailApps: result.options,
                  );
                },
              );
            }
          },
        ),
      ),
    );
  }

  void showNoMailAppsDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text("No Mail Apps Found"),
          content: Text("There are no email apps installed on your device."),
          actions: [
            TextButton(
              child: Text("OK"),
              onPressed: () {
                Navigator.pop(context);
              },
            ),
          ],
        );
      },
    );
  }
}
```

## Key Features

1. Detect Installed Email Apps

- Automatically identifies email apps installed on the device.

2. Open Specific Email App

- Open a specific app based on user selection or app configuration.

3. Native Dialog Support

- On iOS, displays a dialog to let users pick their preferred app when multiple apps are installed.

## Support

Feel free to file issues on the [GitHub](https://github.com/Cuboid-Inc/open_mail) repository for:

- Adding support for additional email apps.
- Reporting bugs or suggesting improvements.

## Contributors

<table>
  <tr>
   <td align="center"><a href="https://github.com/mrcse"><img src="https://avatars.githubusercontent.com/u/73348512?v=4" width="100px;" alt=""/><br /><sub><b>Jamshid Ali</b></sub></a><br /><a href="https://github.com/mrcse" title="Code">💻</a></td>
  <td align="center"><a href="https://github.com/Mubashir-Saeed1"><img src="https://avatars.githubusercontent.com/u/58908265?v=4" width="100px;" alt=""/><br /><sub><b>Mubashir Saeed</b></sub></a><br />
  <a href="https://github.com/Mubashir-Saeed1" title="Code">💻</a></td>
   
  </tr>
</table>

## License

This package is distributed under the MIT License. See the [LICENSE](https://raw.githubusercontent.com/Cuboid-Inc/open_mail/main/LICENSE) file for details.
