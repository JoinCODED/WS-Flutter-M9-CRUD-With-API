We are getting the data from the server. Sometimes this process may take a while to respond, sometimes it returns part of the data earlier than the other. We do not want the `ListView` widget in the home page (that is responsible for displaying the list) to rebuild itself unless the future data is created, and during that period, we need to show a loader.

This where the `FutureBuilder` is used. This Widget builds itself based on the latest snapshot of interaction with a future.

The `FutureBuilder` widget requires two arguments: The first one is the `future` argument (this is the data that we need to wait until it is created), and the second one is the `builder` argument that gives us the `context and dataSnapshot`.

1. In your `home_page.dart`, wrap the consumer widget with the `FutureBuilder` widget as follows :

```dart
            FutureBuilder(
              future:
                  Provider.of<BooksProvider>(context, listen: false).getBooks(),
              builder: (context, dataSnapshot) {
                    return Consumer<BooksProvider>(
                      builder: (context, booksProvider, child) =>
                          ListView.builder(
                              shrinkWrap: true,
                              physics:
                                  const NeverScrollableScrollPhysics(), // <- Here
                              itemCount: booksProvider.books.length,
                              itemBuilder: (context, index) =>
                                  BookCard(book: booksProvider.books[index])),
                    );
                }
            ),
```

We called our provider and accessed the `getBooks` function, and in the `builder` we returned a `Consumer` widget.

Now, we will display a loading spinner to the user just in case the server has internet connection issues:

```dart
FutureBuilder(
              future:
                  Provider.of<BooksProvider>(context, listen: false).getBooks(),
              builder: (context, dataSnapshot) {
                if (dataSnapshot.connectionState == ConnectionState.waiting) {
                  return const Center(
                    child: CircularProgressIndicator(),
                  );
                } else {
                    return Consumer<BooksProvider>(
                      builder: (context, booksProvider, child) =>
                          ListView.builder(
                              shrinkWrap: true,
                              physics:
                                  const NeverScrollableScrollPhysics(), // <- Here
                              itemCount: booksProvider.books.length,
                              itemBuilder: (context, index) =>
                                  BookCard(book: booksProvider.books[index])),
                    );
                  },
              },
            ),
```

`dataSnapshot.connectionState` have two values: `ConnectionState.waiting` and `ConnectionState.done`.

Here, we checked if the status is `waiting`, it will return a widget called `CircularProgressIndicator` which basically is a spinning circle.

We also need to check if an error occurred, in which case we should display a message for the user:

```dart
FutureBuilder(
              future:
                  Provider.of<BooksProvider>(context, listen: false).getBooks(),
              builder: (context, dataSnapshot) {
                if (dataSnapshot.connectionState == ConnectionState.waiting) {
                  return const Center(
                    child: CircularProgressIndicator(),
                  );
                } else {
                  if (dataSnapshot.error != null) {
                    return const Center(
                      child: Text('An error occurred'),
                    );
                  } else {
                    return Consumer<BooksProvider>(
                      builder: (context, booksProvider, child) =>
                          ListView.builder(
                              shrinkWrap: true,
                              physics:
                                  const NeverScrollableScrollPhysics(), // <- Here
                              itemCount: booksProvider.books.length,
                              itemBuilder: (context, index) =>
                                  BookCard(book: booksProvider.books[index])),
                    );
                  }
                }
              },
            ),
```

Test your app, the data must be fetched from the backend.
