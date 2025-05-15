# Library-Management-Project.

About the Project;
1. Creates a database structure for a Library Management System, which helps manage books, authors, members, librarians, and book borrowings.

2. Defines six tables:

   .Authors: Stores information about book authors (like their names).

   .Books: Stores details about books in the library including title, ISBN (unique book code), publication year, and how many copies are available.

   .BookAuthors: A bridge (or join) table that links books and authors, because one book can have multiple authors, and one author can write many books (many-to-many relationship).

   .Members: Stores library member details such as name, email, phone number, and registration date.

   .Librarians: Stores librarian details who manage the lending process.

   .Borrowings: Tracks which member borrows which book, when it was borrowed, when it’s due, and when it was returned. It also records which librarian handled the borrowing.

3. Sets up relationships and constraints:

  .  Primary keys (`PRIMARY KEY`) uniquely identify each record in a table.

  . Foreign keys (`FOREIGN KEY`) link tables together to maintain data integrity, for example, each borrowing record must be linked to an existing book, member, and librarian.

  . Unique constraints (like on `ISBN` and emails) prevent duplicate entries for those fields.

4. Handles data integrity:

    If a book or author is deleted, related records in the `BookAuthors` or `Borrowings` tables are automatically removed (`ON DELETE CASCADE`).

    If a librarian is deleted, borrowing records remain but the librarian reference is set to NULL (`ON DELETE SET NULL`).



 In summary:

This script builds the backbone of a Library Management database. It lets you store, organize, and maintain:

.Who wrote which book,
.What books are available,
. Who the library members are,
.Who the librarians are,
. And who borrowed what book and when.




LIBRARY MANAGEMENT SQL CODES:

-- Drop tables if they already exist (to avoid conflicts)
DROP TABLE IF EXISTS Borrowings;
DROP TABLE IF EXISTS BookAuthors;
DROP TABLE IF EXISTS Books;
DROP TABLE IF EXISTS Authors;
DROP TABLE IF EXISTS Members;
DROP TABLE IF EXISTS Librarians;

-- Create Authors table
CREATE TABLE Authors (
    AuthorID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL
);

-- Create Books table
CREATE TABLE Books (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(255) NOT NULL,
    ISBN VARCHAR(20) UNIQUE NOT NULL,
    PublishedYear YEAR,
    CopiesAvailable INT DEFAULT 1 CHECK (CopiesAvailable >= 0)
);

-- Create BookAuthors table (Many-to-Many)
CREATE TABLE BookAuthors (
    BookID INT NOT NULL,
    AuthorID INT NOT NULL,
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Books(BookID) ON DELETE CASCADE,
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID) ON DELETE CASCADE
);

-- Create Members table
CREATE TABLE Members (
    MemberID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Phone VARCHAR(20),
    RegistrationDate DATE DEFAULT CURRENT_DATE
);

-- Create Librarians table
CREATE TABLE Librarians (
    LibrarianID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL
);

-- Create Borrowings table
CREATE TABLE Borrowings (
    BorrowingID INT AUTO_INCREMENT PRIMARY KEY,
    BookID INT NOT NULL,
    MemberID INT NOT NULL,
    BorrowDate DATE NOT NULL,
    DueDate DATE NOT NULL,
    ReturnDate DATE,
    LibrarianID INT,
    FOREIGN KEY (BookID) REFERENCES Books(BookID) ON DELETE CASCADE,
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID) ON DELETE CASCADE,
    FOREIGN KEY (LibrarianID) REFERENCES Librarians(LibrarianID) ON DELETE SET NULL
);
