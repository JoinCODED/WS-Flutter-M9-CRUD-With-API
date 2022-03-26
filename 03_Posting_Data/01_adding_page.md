Now we will learn how to send a new book to the backend to be saved in the database.

First step is to create `add_page.dart` file in the `pages` folder.

```dart
class AddPage extends StatelessWidget {
  AddPage({Key? key}) : super(key: key);
  final titleController = TextEditingController();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Add a book"),
      ),
      resizeToAvoidBottomInset: false,
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          children: [
            const Text("Fill those field to add a book"),
          ],
        ),
      ),
    );
  }
}
```

And we should include this route in our routes in `main.dart`:

```dart
  final _router = GoRouter(
    routes: [
      GoRoute(
        path: '/',
        builder: (context, state) => const HomePage(),
      ),
      GoRoute(
        path: '/add',
        builder: (context, state) => AddPage(),
      ),
    ],
  );
```

Then in your `home_page.dart` point the `add a new book` button to the page we just created:

```dart
    child: ElevatedButton(
        onPressed: () {
        GoRouter.of(context).push('/add');
},
```

We used to do `TextField`s, but this time we will create a `Form` widget, and also we will learn whats the difference and why we use `Forms`.
