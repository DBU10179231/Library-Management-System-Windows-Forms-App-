Library Management System (Windows Forms App)
This is a basic Windows Forms application designed to manage a library system, including user authentication, and sections for managing books and borrowers. It connects to a MySQL database for data storage.

Features
User Authentication: Secure login system with username and password verification.

Login History: Logs all login attempts (successful and failed) to the database for auditing.

Books Management: (Placeholder) Tab for managing library books, likely including adding, editing, and deleting books.

Borrowers Management: (Placeholder) Tab for managing library borrowers, likely including adding, editing, and deleting borrowers.

MySQL Database Integration: Uses MySQL for persistent data storage.

Prerequisites
Before you begin, ensure you have the following installed:

Visual Studio: Any recent version (e.g., 2019, 2022) with .NET desktop development workload.

MySQL Server: A running instance of MySQL Database Server.

MySQL.Data NuGet Package: This ADO.NET provider is necessary for C# applications to connect to MySQL.

In Visual Studio, right-click on your project in Solution Explorer -> Manage NuGet Packages...

Go to the Browse tab, search for MySql.Data, and install the latest stable version.

Database Setup (MySQL)
You need to create the LibrarySystem database and the necessary tables in your MySQL server.

Connect to your MySQL Server: Use a tool like MySQL Workbench, DBeaver, or the MySQL command-line client.

Create the Database:

CREATE DATABASE LibrarySystem;
USE LibrarySystem;

Create the users table:
This table stores user credentials for login.

CREATE TABLE users (
    Username VARCHAR(50) PRIMARY KEY,
    Password VARCHAR(50) NOT NULL
);

-- Insert some example users for testing
INSERT INTO users (Username, Password) VALUES ('admin', 'password');
INSERT INTO users (Username, Password) VALUES ('user1', 'pass123');

Create the LoginHistory table:
This table logs all login attempts.

CREATE TABLE LoginHistory (
    Id INT AUTO_INCREMENT PRIMARY KEY,
    Username VARCHAR(50) NOT NULL,
    LoginTime DATETIME DEFAULT CURRENT_TIMESTAMP,
    Status VARCHAR(100) NOT NULL
);

Create the Books table:
This table stores information about books in the library.

CREATE TABLE Books (
    BookID INT AUTO_INCREMENT PRIMARY KEY,
    Title VARCHAR(255) NOT NULL,
    Author VARCHAR(255),
    ISBN VARCHAR(20) UNIQUE,
    PublicationYear INT,
    AvailableCopies INT DEFAULT 0
);

-- Example books
INSERT INTO Books (Title, Author, ISBN, PublicationYear, AvailableCopies) VALUES
('The Great Gatsby', 'F. Scott Fitzgerald', '978-0743273565', 1925, 5),
('1984', 'George Orwell', '978-0451524935', 1949, 3);

Create the Borrowers table:
This table stores information about library borrowers.

CREATE TABLE Borrowers (
    BorrowerID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    ContactInfo VARCHAR(255),
    Address VARCHAR(255)
);

-- Example borrowers
INSERT INTO Borrowers (Name, ContactInfo, Address) VALUES
('Alice Smith', 'alice@example.com', '123 Main St'),
('Bob Johnson', 'bob@example.com', '456 Oak Ave');

Create the IssuedBooks table:
This table tracks which books are issued to which borrowers.

CREATE TABLE IssuedBooks (
    IssueID INT AUTO_INCREMENT PRIMARY KEY,
    BookID INT NOT NULL,
    BorrowerID INT NOT NULL,
    IssueDate DATE NOT NULL,
    DueDate DATE NOT NULL,
    ReturnDate DATE, -- Null if not yet returned
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (BorrowerID) REFERENCES Borrowers(BorrowerID)
);

-- Example issued book (assuming BookID 1 for 'The Great Gatsby' and BorrowerID 1 for 'Alice Smith')
INSERT INTO IssuedBooks (BookID, BorrowerID, IssueDate, DueDate, ReturnDate) VALUES
(1, 1, '2023-01-10', '2023-01-24', NULL);

Application Setup
Create a New Project:

Open Visual Studio.

Select "Create a new project".

Choose "Windows Forms App (.NET Framework)" or "Windows Forms App" (for .NET Core) and click Next.

Name your project LibraryMgtSystem.

Rename Form1.cs to Loginform.cs:

In Solution Explorer, right-click on Form1.cs and choose "Rename".

Type Loginform.cs and press Enter. Click "Yes" when prompted to rename all references.

Add Main_Window.cs:

In Solution Explorer, right-click on your project (LibraryMgtSystem) -> Add -> New Item...

Select "Windows Form" and name it Main_Window.cs. Click "Add".

Update Code Files:

Replace the entire content of your Loginform.cs file with the code provided in the Loginform.cs (Updated for MySQL) Canvas.

Replace the entire content of your Main_Window.cs file with the code provided in the Main_Window.cs (Final Fix for button6_Click) Canvas.

Design Your Forms (Visual Studio Designer):

Loginform.cs [Design]:

Add a TextBox named textBox1 (for Username).

Add a TextBox named textBox2 (for Password).

Add a Button named button1 (Text: "Login").

Add a Label named label3 (for error messages; set its ForeColor property to Red).

(Optional) Add Labels for "Username" (label1) and "Password" (label2).

Main_Window.cs [Design]:

Add a TabControl named tabControl1.

Inside tabControl1, add two TabPages:

First TabPage: Set its Name to tabPage1 and Text to "Books Management".

Inside tabPage1, add a DataGridView named dataGridView1.

Add buttons named button1 (Text: "AddBook"), button2 (Text: "EditBook"), button3 (Text: "DeleteBook").

Second TabPage: Set its Name to tabPage2 and Text to "Borrowers Management".

Inside tabPage2, add a DataGridView named dataGridView2.

Add buttons named button4 (Text: "Add Borrower"), button5 (Text: "Edit Borrower"), button6 (Text: "Delete Borrower").

Add a Label named labelWelcome directly on the Main_Window form (outside the TabControl). This label will display the welcome message.

Usage
Run the Application:

In Visual Studio, press F5 or click the "Start" button.

The Loginform will appear.

Login:

Use the example credentials: Username: admin, Password: password.

Click the "Login" button.

If successful, the Main_Window will open, displaying a welcome message and the tabbed interface.

Navigate Tabs:

Click on the "Books Management" tab to see the list of books (if LoadBooksData is fully implemented to fetch data).

Click on the "Borrowers Management" tab to see the list of borrowers (if LoadBorrowersData is fully implemented to fetch data).

Clicking the placeholder buttons will show a MessageBox indicating their intended function.

Configuration
Database Connection String
The database connection string is defined in both Loginform.cs and Main_Window.cs:

private string connectionString = "Server=localhost;Database=LibrarySystem;Uid=admin;Pwd=admin123;";

Server: Replace localhost with the IP address or hostname of your MySQL server.

Database: Ensure this is LibrarySystem or whatever you named your database.

Uid: Your MySQL username (e.g., admin).

Pwd: Your MySQL password (e.g., admin123).

Make sure the connection string is identical in both Loginform.cs and Main_Window.cs for consistent database access.

Extending Functionality
Implement CRUD Operations: Fill in the LoadBooksData, LoadBorrowersData methods with actual data retrieval queries. Implement the logic for Add, Edit, and Delete buttons on both management tabs.

Issued Books Overview: You can add a third tab for "Issued Books Overview" and integrate the LoadIssuedBooksOverviewData method into it, ensuring you add a DataGridView (e.g., dataGridView3) to that new tab.

Error Handling: Enhance error handling with more specific user feedback or logging mechanisms.

UI/UX Improvements: Improve the user interface and experience with better layouts, validation feedback, and visual styling.

Search/Filter: Add search and filter capabilities to the DataGridViews.

Reporting: Implement features for generating reports (e.g., overdue books).
