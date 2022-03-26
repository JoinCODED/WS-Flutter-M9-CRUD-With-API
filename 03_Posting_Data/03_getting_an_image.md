To get an image from the user gallery we will first install the `image_picker` package:

```shell
flutter pub add image_picker
```

Then we'll import our file in the `add_form.dart`.

```dart
import 'package:image_picker/image_picker.dart';
```

On the top of our widget, we will create a variable to hold our image, and we will initialize an `ImagePicker` instance:

```dart
  var _image;
  final _picker = ImagePicker();
```

Within our form after the text fields, Create a `Row` with a `GestureDetector` widget inside and a `Text` widget:

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

Now within our `onTap` method, we will paste the following code:

```dart
onTap: () {
    final XFile? image = await _picker.pickImage(source: ImageSource.gallery);
    setState(() {
    _image = File(image!.path);
    });
}
```

We need to mark the method as `async`, because we need to `await` the `pickImage` method.

You can change the source of the image from `ImageSource.gallery` to `ImageSource.camera` if you wish like the user will take a picture.

Then we called the `setState` method to store the image path for the image the user selected inside `_image` variable.

We have one issue though, how can we tell the user that he picked an image? it would be nice if we show him the image he picked.

Inside our `Container`, we will check if the user selected an image and if so, we will display this image using the `Image.File` widget:

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

That's it, we can now fill our fields, pick an image, what's left is to create the `dio` method responsible for sending those values to the backend.
