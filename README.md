import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

// Represents a single book in the library
class Book {
    private int id;
    private String title;
    private String author;
    private boolean isIssued;

    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.isIssued = false; // A new book is always available
    }

    // Getters and Setters
    public int getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public boolean isIssued() {
        return isIssued;
    }

    public void issueBook() {
        this.isIssued = true;
    }

    public void returnBook() {
        this.isIssued = false;
    }

    @Override
    public String toString() {
        return "ID: " + id + ", Title: '" + title + "', Author: '" + author + "', Status: "
                + (isIssued ? "Issued" : "Available");
    }
}

// Manages all library operations
class Library {
    private List<Book> books;
    private int nextBookId = 1;

    public Library() {
        this.books = new ArrayList<>();
    }

    // Add a new book to the library
    public void addBook(String title, String author) {
        Book newBook = new Book(nextBookId++, title, author);
        books.add(newBook);
        System.out.println("Book added successfully: " + newBook);
        // --- JDBC Code Placeholder ---
        // try (Connection conn = DatabaseManager.getConnection()) {
        //     String sql = "INSERT INTO books (title, author, is_issued) VALUES (?, ?, ?)";
        //     PreparedStatement pstmt = conn.prepareStatement(sql);
        //     pstmt.setString(1, title);
        //     pstmt.setString(2, author);
        //     pstmt.setBoolean(3, false);
        //     pstmt.executeUpdate();
        // } catch (SQLException e) { e.printStackTrace(); }
    }

    // Find a book by its ID
    public Book findBookById(int id) {
        return books.stream()
                .filter(book -> book.getId() == id)
                .findFirst()
                .orElse(null);
    }

    // Issue a book
    public void issueBook(int id) {
        Book book = findBookById(id);
        if (book != null) {
            if (!book.isIssued()) {
                book.issueBook();
                System.out.println("Book issued successfully: " + book.getTitle());
                // --- JDBC Code Placeholder ---
                // Update the book's status in the database to issued
            } else {
                System.out.println("Sorry, this book is already issued.");
            }
        } else {
            System.out.println("Invalid book ID.");
        }
    }

    // Return a book
    public void returnBook(int id) {
        Book book = findBookById(id);
        if (book != null) {
            if (book.isIssued()) {
                book.returnBook();
                System.out.println("Book returned successfully: " + book.getTitle());
                 // --- JDBC Code Placeholder ---
                // Update the book's status in the database to available
            } else {
                System.out.println("This book was not issued.");
            }
        } else {
            System.out.println("Invalid book ID.");
        }
    }

    // Display all books in the library
    public void displayAllBooks() {
        System.out.println("\n--- Library Collection ---");
        if (books.isEmpty()) {
            System.out.println("The library is currently empty.");
        } else {
            books.forEach(System.out::println);
        }
        System.out.println("------------------------\n");
    }
}

// Main class to run the console application
public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        Scanner scanner = new Scanner(System.in);

        // Sample data
        library.addBook("The Great Gatsby", "F. Scott Fitzgerald");
        library.addBook("To Kill a Mockingbird", "Harper Lee");
        library.addBook("1984", "George Orwell");

        int choice;
        do {
            System.out.println("\n--- Library Management System Menu ---");
            System.out.println("1. Add a new book");
            System.out.println("2. Issue a book");
            System.out.println("3. Return a book");
            System.out.println("4. Display all books");
            System.out.println("0. Exit");
            System.out.print("Enter your choice: ");

            choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter book title: ");
                    String title = scanner.nextLine();
                    System.out.print("Enter book author: ");
                    String author = scanner.nextLine();
                    library.addBook(title, author);
                    break;
                case 2:
                    System.out.print("Enter the book ID to issue: ");
                    int issueId = scanner.nextInt();
                    library.issueBook(issueId);
                    break;
                case 3:
                    System.out.print("Enter the book ID to return: ");
                    int returnId = scanner.nextInt();
                    library.returnBook(returnId);
                    break;
                case 4:
                    library.displayAllBooks();
                    break;
                case 0:
                    System.out.println("Exiting the application. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 0);

        scanner.close();
    }
}
