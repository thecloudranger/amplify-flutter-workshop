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

### Gallery Page

Let's create the gallery view page that allows you to view images. Add **image_upload_page.dart** with the following content : 

``` dart

import 'package:flutter/material.dart';

class GalleryView extends StatefulWidget {
  final VoidCallback didSignOut;

  GalleryView({Key key, @required this.didSignOut}) : super(key: key);

  @override
  State<StatefulWidget> createState() => _GalleryViewState();
}

class _GalleryViewState extends State<GalleryView> {
  List<dynamic> _photos = [];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Gallery'),
      ),
      body: Builder(builder: (context) {
        if (_photos.isEmpty) {
          return Center(
            child: Text('No photos uploaded'),
          );
        } else {
          return GridView.builder(
              gridDelegate:
                  SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 2),
              itemBuilder: (context, index) {
                return ListTile(
                  title: Icon(Icons.photo),
                );
              });
        }
      }),
      floatingActionButton: FloatingActionButton(
          child: Icon(Icons.photo_album),
          onPressed: () {
            showModalBottomSheet(context: context, builder: (context) {});
          }),
    );
  }
}

```

### Update Home page to access the Gallery page

Update the **home_page.dart** to access the Gallery page

```dart
import 'dart:io';

import 'package:amplify_flutter/amplify.dart';
import 'package:amplify_auth_cognito/amplify_auth_cognito.dart';
import 'package:flutter/material.dart';
import 'image_upload_page.dart';

class HomePage extends StatelessWidget {
  // 2
  final VoidCallback shouldLogOut;

  HomePage({Key key, this.shouldLogOut}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return FutureBuilder(
      future: Amplify.Auth.getCurrentUser(),
      builder: (BuildContext context, AsyncSnapshot<AuthUser> snapshot) {
        final currentUser = snapshot.data;
        return Scaffold(
          appBar: AppBar(
            title: Text("Home"),
            actions: [
              // Log Out Button
              Padding(
                padding: const EdgeInsets.all(8),
                child: GestureDetector(
                    child: Icon(Icons.logout), onTap: shouldLogOut),
              )
            ],
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text("AWS Amplify"),
                Text("User ID ${currentUser?.userId}"),
                Text("User Name ${currentUser?.username}"),
                OutlineButton(
                  child: Text("Gallery View"),
                  onPressed: () => _gotoGalleryScreen(context),
                  color: Theme.of(context).primaryColor,
                ),
              ],
            ),
          ),
        );
      },
    );
  }

  void _gotoGalleryScreen(BuildContext context) {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (_) => GalleryView(),
      ),
    );
  }    
}

```

### Configuring Storage for Gallery

We'll start by importing the dependencies into **image_upload_page.dart**

``` dart
...// import 'package:flutter/material.dart'; (line 1)
import 'dart:io';
import 'package:amplify_flutter/amplify.dart';
import 'package:amplify_storage_s3/amplify_storage_s3.dart';
import 'package:cached_network_image/cached_network_image.dart';
import 'package:file_picker/file_picker.dart';
import 'package:flutter/material.dart';

...// class GalleryView extends StatefulWidget {
```
Add the functions _getImageUrls and _pickAndUploadImage

``` dart
... // } Closing brace of Widget build(BuildContext context)  (line 48)
  //1
  void _getImageUrls() async {
    try {
      final listOptions =
          S3ListOptions(accessLevel: StorageAccessLevel.private);
      //2
      final listResult = await Amplify.Storage.list(options: listOptions);
      print(listResult.items);
      final getUrlOptions =
          GetUrlOptions(accessLevel: StorageAccessLevel.private);
      //3
      final imageUrls = await Future.wait(listResult.items.map((item) async {
        final urlResult =
            await Amplify.Storage.getUrl(key: item.key, options: getUrlOptions);
        return urlResult.url;
      }));

      setState(() {
        _imageUrls = imageUrls;
      });
    } catch (e) {
      print(e);
    }
  }
  //4
  void _pickAndUploadImage() async {
    final selectedFileResult =
        await FilePicker.platform.pickFiles(allowCompression: true);

    if (selectedFileResult != null) {
      print(selectedFileResult.files);      
      final selectedImageFile = File(selectedFileResult.files.single.path);
      final key = '${DateTime.now()}.jpg';
      //5
      try {
        final uploadFileOptions =
            UploadFileOptions(accessLevel: StorageAccessLevel.private);
        await Amplify.Storage.uploadFile(
            local: selectedImageFile, key: key, options: uploadFileOptions);
        print('image uploaded');
        _getImageUrls();
      } catch (e) {
        print(e);
      }
    }
  }

```
1. The function _getImageUrls contains the logic for fetching the image URLs.
2. You can list all of the objects uploaded under a given prefix. For the user to read the file, specify the same access level (private) and key you used to upload.
3. Fetch the image URLs for every key obtained.
4. The function _pickAndUploadImage holds the logic for selecting an image and uploading it to S3.
5. After selecting a file, to upload to S3 from a data object, specify the key and the file to be uploaded.

When adding the Storage category, you configure the level of access users have to your S3 bucket. You can configure separate rules for authenticated vs. guest users. When using the Storage category to upload files, you can also specify an access level for each individual file: guest, protected, or private.

* **Guest** Accessible by all users of your application
* **Protected** Readable by all users, but only writable by the creating user
* **Private** Readable and writable only by the creating user

In the above code snippet an options object specifying the private access level to only allow an object to be accessed by the creating user is created.

Update the list name _photos to _imageUrls to reflect fetching of image URLs from S3. Replace the same in other references in **image_upload_page.dart**

``` dart
...// class _GalleryViewState extends State<GalleryView> { (line 18)
  List<String> _imageUrls = [];
```

Replace the GridView with a refresh indicator that will fetch the image URLs and create a GridView to list the images

```dart
... // return GridView.builder( (line 32)
          return RefreshIndicator(
            onRefresh: () async => _getImageUrls(),
            child: GridView.builder(
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2, crossAxisSpacing: 4, mainAxisSpacing: 4),
                itemCount: _imageUrls.length,
                itemBuilder: (context, index) {
                  return CachedNetworkImage(
                    imageUrl: _imageUrls[index],
                    placeholder: (context, url) =>
                        Center(child: CircularProgressIndicator()),
                    fit: BoxFit.cover,
                    width: double.infinity,
                    height: double.infinity,
                  );
                }),
          );
```

Set onPressed to invoke the function _pickAndUploadImage

```dart
... // onPressed: () { (line 53)
          onPressed: () => _pickAndUploadImage()),
```

Initialize the state 

``` dart
... // List<String> _imageUrls = []; (line 18)
  @override
  void initState() {
    super.initState();
    _getImageUrls();
  }
```

Build and run the app and you should be able to upload images and view them in the gallery.

[<- Module 3](../module3/README.md) || [^ Navigate Home ^](../README.md)