To get an image from the user gallery, we can use a library called [image_picker](https://pub.dev/packages/image_picker).

1. Install the `image_picker` package:

```shell
flutter pub add image_picker
```

2. Import it in the `add_form.dart`:

```dart
import 'package:image_picker/image_picker.dart';
```

3. At the top of the widget, create a variable to store the image, and initialize an `ImagePicker` instance:

```dart
  var _image;
  final _picker = ImagePicker();
```

4. Inside the form after the text fields, create a `Row` with a `GestureDetector` widget, and a `Text` widget:

```dart
      Row(
        children: [
          GestureDetector(
            onTap: () {},
            child: Container(
              width: 100,
              height: 100,
              margin: const EdgeInsets.only(top: 20),
              decoration: BoxDecoration(color: Colors.blue[200]),
              child:Container(
                      decoration: BoxDecoration(color: Colors.blue[200]),
                      width: 200,
                      height: 200,
                      child: Icon(
                        Icons.camera_alt,
                        color: Colors.grey[800],
                      ),
                ),
            ),
          ),
          const Padding(
            padding: EdgeInsets.all(8.0),
            child: Text("Image"),
          )
        ],
      )
```

5. In the `onTap` method, paste the following code:

```dart
    onTap: () {
      final XFile? image = await _picker.pickImage(source: ImageSource.gallery);
      setState(() {
      _image = File(image!.path);
      });
    }
```

We marked it as `async`, because the `pickImage` method is asynchronous.

You can change the source of the image from `ImageSource.gallery` to `ImageSource.camera` if you would like the user to take a picture using his camera.

We called the `setState` method to store the path of the image that the user selected inside the `_image` variable.

We have one issue though, how can we tell the user that he picked an image? It would be nice to tell him by displaying the picked image on the screen.

6. Inside the `Container`, check if the user selected an image. If so, we will display this image using the `Image.File` widget:

```dart
child: Container(
                width: 100,
                height: 100,
                margin: const EdgeInsets.only(top: 20),
                decoration: BoxDecoration(color: Colors.blue[200]),
                child: _image != null
                    ? Image.file(
                        _image,
                        width: 200.0,
                        height: 200.0,
                        fit: BoxFit.fitHeight,
                      )
                    : Container(
                        decoration: BoxDecoration(color: Colors.blue[200]),
                        width: 200,
                        height: 200,
                        child: Icon(
                          Icons.camera_alt,
                          color: Colors.grey[800],
                        ),
                      ),
              ),
```

Now, we can fill the fields and pick an image.

One thing is left to do, we need to create a `dio` method responsible for sending those values to the backend.
