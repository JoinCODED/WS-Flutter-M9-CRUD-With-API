In this section, we will learn how to send a new book to the backend to be saved in the database.

1. Create a new file named `add_page.dart` in the `pages` folder, and set it up as follows:

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

2. Create a route for the `add_page.dart` file in the `main.dart`:

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

3. In the `home_page.dart` file, point the `add a new book` button to the page we just created (`add_page.dart`):

```dart
    child: ElevatedButton(
        onPressed: () {
        GoRouter.of(context).push('/add');
},
```
