We have `ListView.builder` and `GridView.builder` that asks us to provide them with items and items count, but we don't have that, the server may return 100 books or may return 5, so to solve this we will wrap our `ConsumerWidget` with a `FutureBuilder`:

In your `home_page.dart`:

```dart
Consumer<BooksProvider>(
    builder: (context, booksProvider, child) =>
            ListView.builder(
                shrinkWrap: true,
                physics:
                    const NeverScrollableScrollPhysics(), // <- Here
                itemCount: booksProvider.books.length,
                itemBuilder: (context, index) =>
                BookCard(book: booksProvider.books[index])),
);
```

The `FutureBuilder` widget needs two arguments, the first one is the `future` that we want to wait until it's finished, and a `builder` that gives us `context, dataSnapshot` , the data `dataSnapshot` gives us two important values, let's get to that later, Add a `FutureBuilder` widget and cut your `Consumer` widget as a return for the `builder`.

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

We called our provider and accessed the `getBooks` function and in the `builder` we returned a `Consumer` widget.

Now we will just add some checks, what if the server our the internet connection is slow, we don't want the user to think there's no data, it would be nice if we can show him a loading spinner so he can know something is happening:

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
                  }
                }
              },
            ),
```

`dataSnapshot.connectionState` have two possible values, `ConnectionState.waiting` and `ConnectionState.done`.

So here we checked if the status is `waiting` we will return a widget called `CircularProgressIndicator` that is basically a spinning circle.

One last check we have to do, is if an error occurred, we should display a message for the user:

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

Test your app, That's it we fetched our data from the backend.
