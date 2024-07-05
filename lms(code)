#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <fstream>
#include <cstdlib> 

using namespace std;

class Book {
public:
    int id;
    string title;
    string author;
    bool isIssued;

    Book(int id, string title, string author, bool isIssued = false) {
        this->id = id;
        this->title = title;
        this->author = author;
        this->isIssued = isIssued;
    }
};

class Library {
private:
    vector<Book> books;

    int findBookIndexById(int id) {
        for (size_t i = 0; i < books.size(); ++i) {
            if (books[i].id == id) return i;
        }
        return -1;
    }

public:
    void loadFromFile(const string& filename) {
        ifstream infile(filename);
        if (!infile) {
            cerr << "Could not open file for reading: " << filename << endl;
            return;
        }

        int id;
        string title, author;
        bool isIssued;

        while (infile >> id >> isIssued) {
            infile.ignore(); // Ignore the whitespace
            getline(infile, title);
            getline(infile, author);
            books.emplace_back(id, title, author, isIssued);
        }

        infile.close();
    }

    void saveToFile(const string& filename) {
        ofstream outfile(filename);
        if (!outfile) {
            cerr << "Could not open file for writing: " << filename << endl;
            return;
        }

        for (const auto& book : books) {
            outfile << book.id << " " << book.isIssued << endl;
            outfile << book.title << endl;
            outfile << book.author << endl;
        }

        outfile.close();
    }

    void addBook(Book book) {
        books.push_back(book);
        cout << "Book added successfully." << endl;
    }

    void searchBookByTitle(string title) {
        for (const auto& book : books) {
            if (book.title == title) {
                cout << "Book found: ID: " << book.id << ", Author: " << book.author << ", Status: " << (book.isIssued ? "Issued" : "Available") << endl;
                return;
            }
        }
        cout << "Book not found." << endl;
    }

    void searchBookById(int id) {
        int index = findBookIndexById(id);
        if (index != -1) {
            const auto& book = books[index];
            cout << "Book found: Title: " << book.title << ", Author: " << book.author << ", Status: " << (book.isIssued ? "Issued" : "Available") << endl;
        } else {
            cout << "Book not found." << endl;
        }
    }

    void issueBook(int id) {
        int index = findBookIndexById(id);
        if (index != -1) {
            if (!books[index].isIssued) {
                books[index].isIssued = true;
                cout << "Book issued successfully." << endl;
            } else {
                cout << "Book is already issued." << endl;
            }
        } else {
            cout << "Book not found." << endl;
        }
    }

    void returnBook(int id) {
        int index = findBookIndexById(id);
        if (index != -1) {
            if (books[index].isIssued) {
                books[index].isIssued = false;
                cout << "Book returned successfully." << endl;
            } else {
                cout << "Book was not issued." << endl;
            }
        } else {
            cout << "Book not found." << endl;
        }
    }

    void listAllBooks() {
        sort(books.begin(), books.end(), [](const Book& a, const Book& b) {
            return a.id < b.id;
        });

        cout << "\nListing All Books:\n";
        for (const auto& book : books) {
            cout << "ID: " << book.id << ", Title: " << book.title << ", Author: " << book.author << ", Status: " << (book.isIssued ? "Issued" : "Available") << endl;
        }
        cout << "\n";
    }

    void deleteBook(int id) {
        int index = findBookIndexById(id);
        if (index != -1) {
            books.erase(books.begin() + index);
            cout << "Book deleted successfully." << endl;
        } else {
            cout << "Book not found." << endl;
        }
    }
};

void displayMenu() {
    cout << "Library Management System\n";
    cout << "1. Add Book\n";
    cout << "2. Search Book by Title\n";
    cout << "3. Search Book by ID\n";
    cout << "4. Issue Book\n";
    cout << "5. Return Book\n";
    cout << "6. List All Books\n";
    cout << "7. Delete Book\n";
    cout << "8. Exit\n";
    cout << "Enter your choice: ";
}

int main() {
    Library library;
    string filename = "famous_books.txt";

    library.loadFromFile(filename);

    int choice;
    bool exitProgram = false;
    while (!exitProgram) {
        displayMenu();
        cin >> choice;

        int id;
        string title, author;

        switch (choice) {
            case 1:
                cout << "Enter Book ID: ";
                cin >> id;
                cout << "Enter Book Title: ";
                cin.ignore();
                getline(cin, title);
                cout << "Enter Book Author: ";
                getline(cin, author);
                library.addBook(Book(id, title, author));
                break;
            case 2:
                cout << "Enter Book Title: ";
                cin.ignore();
                getline(cin, title);
                library.searchBookByTitle(title);
                break;
            case 3:
                cout << "Enter Book ID: ";
                cin >> id;
                library.searchBookById(id);
                break;
            case 4:
                cout << "Enter Book ID: ";
                cin >> id;
                library.issueBook(id);
                break;
            case 5:
                cout << "Enter Book ID: ";
                cin >> id;
                library.returnBook(id);
                break;
            case 6:
                library.listAllBooks();
                break;
            case 7:
                cout << "Enter Book ID: ";
                cin >> id;
                library.deleteBook(id);
                break;
            case 8:
                library.saveToFile(filename);
                cout << "Exiting and saving data..." << endl;
                exitProgram = true;
                break;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }

        if (!exitProgram) {
            cout << "Do you want to perform another operation? (yes/no): ";
            string response;
            cin.ignore();
            getline(cin, response);
            if (response != "yes" && response != "Yes") {
                library.saveToFile(filename);
                cout << "Exiting and saving data..." << endl;
                exitProgram = true;
            } else {
                cout << "\nPress Enter to continue...\n";
                cin.ignore();
                system("clear"); 
            }
        }
    }

    return 0;
}
