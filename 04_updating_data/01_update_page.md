1. Create a new page responsible for updating books and name it `update_page.dart`. Create a form for the updated data too:

```dart
class UpdatePage extends StatelessWidget {
  final Book book;
  const UpdatePage({Key? key, required this.book}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Update a book"),
      ),
      resizeToAvoidBottomInset: false,
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          children: [
            const Text("Fill those field to update a book"),
            UpdateBookForm(
              book: book,
            ),
          ],
        ),
      ),
    );
  }
}
```

The difference here is that the `update_form.dart` widget expects a book as an argument.
**Note**: The reason that this widget requires a book as an argument is because when the user wants to update a book, he will use a form for that, and since the book has more than one property to be updated, the user might change a specific property and keep the others as they are.
If we do not send the book data to the `update_form.dart`, we cannot fill the fields that the user did not update. Because of that, we cannot tell the backend to keep the rest of the data as it is, so the backend will receive the rest of the fields as empty and will remove the values of the book properties that the user did not fill.

```dart
class UpdateBookForm extends StatefulWidget {
  final Book book;
  UpdateBookForm({required this.book});
  @override
  UpdateFormState createState() {
    return UpdateFormState();
  }
}

class UpdateFormState extends State<UpdateBookForm> {
  final _formKey = GlobalKey<FormState>();
  var _image;
  String title = "";
  String description = "";
  double price = 0;
  final _picker = ImagePicker();
  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book title',
            ),
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
            onSaved: (value) {
              title = value!;
            },
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book Description',
            ),
            maxLines: null,
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
            onSaved: (value) {
              description = value!;
            },
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book price',
            ),
            validator: (value) {
              if (value == null) {
                return "please enter a price";
              } else if (double.tryParse(value) == null) {
                return "please enter a number";
              }
              return null;
            },
            onSaved: (value) {
              price = double.parse(value!);
            },
          ),
          Row(
            children: [
              GestureDetector(
                onTap: () async {
                  final XFile? image =
                      await _picker.pickImage(source: ImageSource.gallery);
                  setState(() {
                    _image = File(image!.path);
                  });
                },
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
              ),
              const Padding(
                padding: EdgeInsets.all(8.0),
                child: Text("Image"),
              )
            ],
          ),
          Center(
            child: ElevatedButton(
              onPressed: () {
                if (_formKey.currentState!.validate()) {
                  _formKey.currentState!.save();
                }
              },
              child: const Text("Update Book"),
            ),
          )
        ],
      ),
    );
  }
}
```
