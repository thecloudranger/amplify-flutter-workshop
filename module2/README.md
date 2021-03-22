# Module 2

## Introduction
With the Flutter app implemented, you can move onto initializing your Amplify project.

**Note** If you are starting from Module 2 directly you can clone the project (main branch) from [here](https://github.com/thecloudranger/amplify-flutter-sample) to continue with the steps below.

At this point, you will need to have the Amplify CLI installed on your machine. Once installed, we will initialize Amplify at the root directory of our project, install the Amplify dependencies into our project, and ensure Amplify is properly configured during each run of our app.

## What You Will Learn
* Initialize a new Amplify project from the command line
* Add Amplify as a dependency for your project
* Initialize Amplify libraries at runtime

## Key Concepts
* Amplify CLI – The Amplify CLI allows you to create, manage, and remove AWS services directly from your terminal.
* Amplify Libraries – The Amplify libraries allow you to interact with AWS services from a web or mobile application.

## Implementation

If you have not already configured your Amplify CLI, be sure to run this:
``` bash
amplify configure
```
You will be guided through the configuration process. You can go Install and configure [here](https://docs.amplify.aws/cli/start/install#option-2-follow-the-instructions) for more information on configuring the CLI.


To create an Amplify project, you must initialize and configure the project at the root directory of your project.

**1.** Navigate to the root of your project:

``` bash
cd workshop_app
```
Verify that you are in the correct directory by running $ ls . Your output should list folders similar to below :

``` bash
❯ ls
README.md                ios                      test
android                  lib                      web
build                    pubspec.lock
flutter_workshop_app.iml pubspec.yaml
```

**2.** Now initialize your project Amplify project by executing:
``` bash
amplify init
```
You should now be prompted with several questions on how to configure your project. If you press the Enter key for each question, it will give the default answer to each question, resulting in an output that should look similar to this:
``` bash
❯ amplify init
Note: It is recommended to run this command from the root of your app directory
? Enter a name for the project flutterworkshopapp
? Enter a name for the environment dev
? Choose your default editor: Visual Studio Code
? Choose the type of app that you\'re building flutter
Please tell us about your project
? Where do you want to store your configuration file? ./lib/
Using default provider  awscloudformation
? Select the authentication method you want to use: AWS profile

For more information on AWS Profiles, see:
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html

? Please choose the profile you want to use amplify-flutter-user
```
After the CLI has finished creating your project in the cloud, you should get an output like this:
``` bash
✔ Successfully created initial AWS cloud resources for deployments.
✔ Initialized provider successfully.
Initialized your environment successfully.

Your project has been successfully initialized and connected to the cloud!
```

**3.** Next, we'll add auth to our amplify app, it's as simple as the below command with a couple of configuration items. Simply run :
``` bash
amplify add auth
```

Answer the questions as per below.
``` bash
 Do you want to use the default authentication and security configuration? Default configuration
 Warning: you will not be able to edit these selections. 
 How do you want users to be able to sign in? Username
 Do you want to configure advanced settings? No, I am done.
 ```

**4.** We're created an Amplify application configuration in our local environment, to get the Amplify CLI to create the requested AWS resources, we use the command :
``` bash
amplify push
```

Be sure to answer yes to continue.
``` bash
Current Environment: dev

| Category | Resource name              | Operation | Provider plugin   |
| -------- | -------------------------- | --------- | ----------------- |
| Auth     | flutterworkshopapp73b82d2c | Create    | awscloudformation |
? Are you sure you want to continue? (Y/n) Y
```

**Awesome, with your AWS resources successfully deployed, you're now ready to commence Module 3!**

[<- Module 1](../module1/README.md) || [^ Navigate Home ^](../README.md) ||  [Module 3 ->](../module3/README.md) 