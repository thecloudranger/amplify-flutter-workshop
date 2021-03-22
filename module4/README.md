# Module 4

## Introduction
In this module you will learn how to upload images and view them in a gallery. 

## What You Will Learn

* Configure Storage
* Gallery view of images
* Upload images
* Show images uploaded by authenticated user

## Key Concepts
Object storage - In an object store, data is stored as objects. Each object consists of a globally unique identifier, some metadata, and the data itself. An object’s globally unique identifier is also known as its key; addressing the object from different devices and machines in a distributed system is possible with the globally unique identifier.

Amazon S3 offers a range of storage classes designed for different use cases
including the following:
* Amazon S3 Standard, for general-purpose storage of frequently accessed data
* Amazon S3 Standard-Infrequent Access (Standard-IA), for long-lived, but less
frequently accessed data
* Amazon Glacier, for low-cost archival data

Check the cloud storage whitepaper for more details [here](https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf).

## Implementation

### Install Amplify Storage

Next, we'll add storage to our amplify app, it's as simple as the below command with a couple of configuration items. Simply run :
``` bash
amplify add storage
```

Answer the questions as per below.
``` bash
? Please select from one of the below mentioned services: Content (Images, audio, video, etc.)
? Please provide a friendly name for your resource that will be used to label this category in the project: S3Bucket
? Please provide bucket name: flutter-workshpop-test-5767593
? Who should have access: Auth users only
? What kind of access do you want for Authenticated users? create/update, read, delete
? Do you want to add a Lambda Trigger for your S3 Bucket? No
Successfully added resource S3Bucket locally
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
| Storage  | S3Bucket                   | Create    | awscloudformation |
| Auth     | flutterworkshopapp73b82d2c | No Change | awscloudformation |
? Are you sure you want to continue? (Y/n) Y
```

### Add dependencies to Flutter
The next step is to install Amplify Storage as a dependency in our project so we can interface with the libraries.

Back in Visual Studio Code, open **pubspec.yaml** and add the following dependency:
``` dart
... // dependencies: (line 23)

  amplify_storage_s3: '<1.0.0'
  file_picker: ^2.1.6
  cached_network_image: ^2.5.0

... // flutter
```

### Configuring Flutter to integrate with Storage
Add the Storage plugin to **main.dart**
``` dart
...// import 'package:amplify_auth_cognito/amplify_auth_cognito.dart'; (line 11)
import 'package:amplify_storage_s3/amplify_storage_s3.dart';

... // void main() {
```

``` dart
.. // AmplifyAuthCognito authPlugin = AmplifyAuthCognito(); (line 91)
    Amplify.addPlugins([authPlugin, storage]);

.. // try {
```

[<- Module 3](../module3/README.md) || [^ Navigate Home ^](../README.md)