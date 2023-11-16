# Library-Management-System
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Structure to represent a book
struct Book {
    string title;
    string author;
    int id;
    bool available;
};

// Structure to represent a user
struct User {
    string name;
    int id;
};

// Structure to represent a transaction (borrowing/returning a book)
struct Transaction {
    int userId;
    int bookId;
    bool borrow; // true if borrowing, false if returning
};

// Function to add a book to the library
void addBook(vector<Book> &library, const string &title, const string &author, int id) {
    Book newBook = {title, author, id, true};
    library.push_back(newBook);
    cout << "Book added successfully!" << endl;
}

// Function to display all books in the library
void displayBooks(const vector<Book> &library) {
    cout << "Library Books:" << endl;
    for (const auto &book : library) {
        cout << "ID: " << book.id << "\tTitle: " << book.title << "\tAuthor: " << book.author
             << "\tStatus: " << (book.available ? "Available" : "Checked Out") << endl;
    }
}

// Function to add a user to the system
void addUser(vector<User> &users, const string &name, int id) {
    User newUser = {name, id};
    users.push_back(newUser);
    cout << "User added successfully!" << endl;
}

// Function to display all users in the system
void displayUsers(const vector<User> &users) {
    cout << "Library Users:" << endl;
    for (const auto &user : users) {
        cout << "ID: " << user.id << "\tName: " << user.name << endl;
    }
}

// Function to handle book transactions (borrowing/returning)
void handleTransaction(vector<Book> &library, vector<User> &users, vector<Transaction> &transactions) {
    int userId, bookId;
    char action;

    cout << "Enter User ID: ";
    cin >> userId;

    // Check if the user exists
    auto user = find_if(users.begin(), users.end(), [userId](const User &u) { return u.id == userId; });

    if (user == users.end()) {
        cout << "User not found!" << endl;
        return;
    }

    cout << "Enter Book ID: ";
    cin >> bookId;

    // Check if the book exists
    auto book = find_if(library.begin(), library.end(), [bookId](const Book &b) { return b.id == bookId; });

    if (book == library.end()) {
        cout << "Book not found!" << endl;
        return;
    }

    cout << "Do you want to borrow (B) or return (R) the book? ";
    cin >> action;

    if (action == 'B' || action == 'b') {
        if (!book->available) {
            cout << "Book is already checked out!" << endl;
        } else {
            book->available = false;
            Transaction newTransaction = {userId, bookId, true};
            transactions.push_back(newTransaction);
            cout << "Book borrowed successfully!" << endl;
        }
    } else if (action == 'R' || action == 'r') {
        if (book->available) {
            cout << "Book is not checked out!" << endl;
        } else {
            book->available = true;
            Transaction newTransaction = {userId, bookId, false};
            transactions.push_back(newTransaction);
            cout << "Book returned successfully!" << endl;
        }
    } else {
        cout << "Invalid action!" << endl;
    }
}

int main() {
    vector<Book> library;
    vector<User> users;
    vector<Transaction> transactions;

    // Example data
    addBook(library, "The Catcher in the Rye", "J.D. Salinger", 1);
    addBook(library, "To Kill a Mockingbird", "Harper Lee", 2);

    addUser(users, "Alice", 101);
    addUser(users, "Bob", 102);

    displayBooks(library);
    displayUsers(users);

    handleTransaction(library, users, transactions);

    // Display updated information
    displayBooks(library);

    return 0;
}
