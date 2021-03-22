# Getting Started

You will need the following in order to execute this workshop

## Flutter 

1. An [editor](https://code.visualstudio.com/) that is compatible with Flutter. This workshop provides prescriptive guidance for Visual Studio Code. For optimal developer experience it is recommended to install `Dart` and `Flutter` [extensions in Visual Studio Code](https://code.visualstudio.com/docs/editor/extension-gallery).

2. Install [Flutter](https://flutter.dev/docs/get-started/install) version 2.0.1 or higher. Only install Flutter and add it to your path ( [Windows](https://flutter.dev/docs/get-started/install/windows#update-your-path) || [macOS](https://flutter.dev/docs/get-started/install/macos#update-your-path)) at this point, further configuration occurs in the following step.

3. Some basic experience with Android / iOS app development (i.e. familiarity with Android Studio interface such as launching Android Emulator).

4. A device [emulator](https://developer.android.com/studio) to run the Flutter application on. Download and install Android Studio and create a device emulator using the instructions for your platform - [macOS](https://flutter.dev/docs/get-started/install/macos#set-up-the-android-emulator) || [Windows](https://flutter.dev/docs/get-started/install/windows#set-up-the-android-emulator)
This Workshop provides prescriptive guidance on working emulating Android as Android Studio supports Windows and macOS development environments.

5. Check flutter configuration using :
``` bash
flutter doctor
```
Your output should look similar to the below :
![Flutter Doctor](./images/flutter-doctor-terminal.png)

## Amplify 

1. An AWS Account with sufficient permission to create AWS IAM, Amazon DynamoDB, Amazon Cognito and AWS Amplify resources. Check [Amplify IAM Policy](https://docs.amplify.aws/cli/usage/iam) for a policy example.
   
    **Note** When attending this workshop during an event organised by AWS, you can choose to use a temporary account for the duration of this workshop. If not done already, follow [these instructions](./event-engine.md) to access a temporary AWS account. 

2. [Install](https://docs.amplify.aws/cli/start/install) Amplify CLI - This is the tool we will use to create and modify the AWS Amplify resources.

3. Configure the Amplify CLI. 
[Watch the video guide](https://docs.amplify.aws/cli/start/install#option-1-watch-the-video-guide) 
or follow [these instructions](https://docs.amplify.aws/cli/start/install#option-2-follow-the-instructions)


**Awesome, you're now ready to commence the workshop!**


[<- Back](../README.md) || [Module 1 ->](../module1/README.md) 
