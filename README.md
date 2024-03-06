[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/sgE29AmB)
# JDBC Assignment 
## Connecting to MySQL Database and Running Basic Queries

---

### Objective:

This assignment aims to introduce students to JDBC (Java Database Connectivity) and guide them through connecting to a MySQL database and executing queries.

### Instructions:

#### Part 1: Setup

1. Download and install MySQL: Install MySQL on your machine if you haven't already. You can download it [here](https://dev.mysql.com/downloads/).

2. Create a Database: Using MySQL Workbench or any MySQL client, create a new database named `university`.

3. Create a Table: Within the `university` database, create a table named `students` with the following columns:
   - `id` (INT, PRIMARY KEY, AUTO_INCREMENT)
   - `name` (VARCHAR)
   - `age` (INT)

#### Part 2: Java Program

Write a Java program that connects to the MySQL database and performs the following operations:

1. **Connect to the Database:**
   - Use JDBC to establish a connection to the MySQL database.

2. **Insert Data:**
   - Insert at least two records into the `students` table.

3. **Retrieve Data:**
   - Retrieve and print all records from the `students` table.

4. **Update Data:**
   - Update the age of one student.

5. **Delete Data:**
   - Delete one student from the table.

#### Part 3: Submission

Submit the Java source code file along with a screenshot of the table you created using [MySQL Shell](https://www.mysqltutorial.org/mysql-administration/mysql-show-columns/) in the same way you submitted your last assignment.

### Code:-
import java.sql.*;
import java.util.Scanner;

public class Assignment1 {
    // JDBC URL, username, and password of MySQL server
    static final String JDBC_URL = "jdbc:mysql://localhost:3306/university";
    static final String USERNAME = "root";
    static final String PASSWORD = "*******";

    public static void main(String[] args) {
        try {
            // Creating an instance of Assignment2 to access non-static methods
            Assignment1 assignment = new Assignment1();

            // Establishing a connection to the database
            Connection connection = assignment.getConnection();
            System.out.println("Connected to the database");

            // Taking input from the user
            Scanner scanner = new Scanner(System.in);
            System.out.print("Enter the number of entries you want to make: ");
            int numEntries = scanner.nextInt();
            scanner.nextLine(); // Consume newline left-over

            for (int i = 0; i < numEntries; i++) {
                System.out.println("Entry " + (i + 1) + ":");
                System.out.print("Enter student name: ");
                String name = scanner.nextLine();
                System.out.print("Enter student age: ");
                int age = scanner.nextInt();
                scanner.nextLine(); // Consume newline left-over

                // Inserting data into the students table
                assignment.insertData(connection, name, age);
            }

            // Retrieving and printing all records from the students table
            assignment.retrieveData(connection);

            // Updating the attributes of one student
            assignment.updateData(connection);

            // Deleting one student from the table
            assignment.deleteData(connection);

            // Closing the connection
            connection.close();
            System.out.println("Connection closed");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Method to establish a database connection
    public Connection getConnection() throws SQLException {
        return DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD);
    }

    // Method to insert data into the students table
    public void insertData(Connection connection, String name, int age) throws SQLException {
        PreparedStatement statement = connection.prepareStatement("INSERT INTO students (name, age) VALUES (?, ?)");
        statement.setString(1, name);
        statement.setInt(2, age);
        statement.executeUpdate();
        System.out.println("Data inserted into students table");
    }

    // Method to retrieve and print all records from the students table
    public void retrieveData(Connection connection) throws SQLException {
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery("SELECT * FROM students");
        System.out.println("Retrieving data from students table:");
        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("name");
            int age = resultSet.getInt("age");
            System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age);
        }
    }

    // Method to update the attributes of one student
    public void updateData(Connection connection) throws SQLException {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the ID of the student to update: ");
        int id = scanner.nextInt();
        scanner.nextLine(); // Consume newline left-over

        System.out.print("Enter updated name: ");
        String updatedName = scanner.nextLine();
        System.out.print("Enter updated age: ");
        int updatedAge = scanner.nextInt();
        scanner.nextLine(); // Consume newline left-over

        PreparedStatement statement = connection.prepareStatement("UPDATE students SET name = ?, age = ? WHERE id = ?");
        statement.setString(1, updatedName);
        statement.setInt(2, updatedAge);
        statement.setInt(3, id);
        int rowsUpdated = statement.executeUpdate();
        if (rowsUpdated > 0) {
            System.out.println("Attributes of one student updated");
        } else {
            System.out.println("No student found with the given ID");
        }
    }

    // Method to delete one student from the table
    public void deleteData(Connection connection) throws SQLException {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the ID of the student to delete: ");
        int id = scanner.nextInt();

        Statement statement = connection.createStatement();
        // Deleting the student with the provided ID
        int rowsDeleted = statement.executeUpdate("DELETE FROM students WHERE id = " + id);
        if (rowsDeleted > 0) {
            System.out.println("One student deleted from the table");
        } else {
            System.out.println("No student found with the given ID");
        }
    }
}



### OUTPUT:-
![image](https://github.com/CodeX-SIT/jdbc-assignment-Jayu1214/assets/91301490/43557acc-cb4b-4289-bdab-f6b221468a3f)

![image](https://github.com/CodeX-SIT/jdbc-assignment-Jayu1214/assets/91301490/5b3db726-b668-4f01-a5ef-edea5c762b13)


### Additional Tips:

- Use the JDBC documentation ([Oracle JDBC API](https://docs.oracle.com/en/java/javase/14/docs/api/java.sql/java/sql/package-summary.html)) as a reference guide.
- Ensure you have the MySQL Connector/J library in your project. You can download it [here](https://dev.mysql.com/downloads/connector/j/).
- If you face any issues, you can contact your technical executives and peers in the community!
