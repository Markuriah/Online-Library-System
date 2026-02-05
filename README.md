#include <iostream>
#include <vector>
using namespace std;

// ---------- Book ----------
class Book {
private:
    int id;
    string title;
    string author;
    bool isBorrowed;

public:
    Book(int id, string title, string author)
        : id(id), title(title), author(author), isBorrowed(false) {}

    int getId() { return id; }
    string getTitle() { return title; }
    string getAuthor() { return author; }
    bool getStatus() { return isBorrowed; }

    void borrowBook() { isBorrowed = true; }
    void returnBook() { isBorrowed = false; }
};

// ---------- User ----------
class User {
private:
    int userId;
    string name;
    vector<int> borrowedBooks;

public:
    User(int userId, string name)
        : userId(userId), name(name) {}

    int getUserId() { return userId; }
    string getName() { return name; }

    void borrowBook(int bookId) {
        borrowedBooks.push_back(bookId);
    }

    void returnBook(int bookId) {
        for (auto it = borrowedBooks.begin(); it != borrowedBooks.end(); it++) {
            if (*it == bookId) {
                borrowedBooks.erase(it);
                break;
            }
        }
    }

    vector<int> getBorrowedBooks() {
        return borrowedBooks;
    }
};

// ---------- Library ----------
class Library {
private:
    vector<Book> books;
    vector<User> users;

public:
    void addBook(Book book) {
        books.push_back(book);
    }

    void addUser(User user) {
        users.push_back(user);
    }

    Book* searchBookById(int bookId) {
        for (auto &book : books) {
            if (book.getId() == bookId)
                return &book;
        }
        return nullptr;
    }

    User* searchUserById(int userId) {
        for (auto &user : users) {
            if (user.getUserId() == userId)
                return &user;
        }
        return nullptr;
    }

    void borrowBook(int bookId, int userId) {
        Book* book = searchBookById(bookId);
        User* user = searchUserById(userId);

        if (book && user && !book->getStatus()) {
            book->borrowBook();
            user->borrowBook(bookId);
            cout << "Book borrowed successfully\n";
        } else {
            cout << "Borrowing failed\n";
        }
    }

    void returnBook(int bookId, int userId) {
        Book* book = searchBookById(bookId);
        User* user = searchUserById(userId);

        if (book && user && book->getStatus()) {
            book->returnBook();
            user->returnBook(bookId);
            cout << "Book returned successfully\n";
        } else {
            cout << "Return failed\n";
        }
    }
};

// ---------- Main ----------
int main() {
    Library library;

    library.addBook(Book(1, "C++ Programming", "Bjarne Stroustrup"));
    library.addBook(Book(2, "Data Structures", "Mark Allen"));

    library.addUser(User(101, "Paul"));
    library.addUser(User(102, "Alice"));

    library.borrowBook(1, 101);
    library.returnBook(1, 101);

    return 0;
}
