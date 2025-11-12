# Student-management-system-
Creating a simple student management system 
import sqlite3

# Connect to database (or create)
conn = sqlite3.connect('students.db')
cursor = conn.cursor()

# Create table
cursor.execute('''
CREATE TABLE IF NOT EXISTS students(
    roll_no TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    marks INTEGER NOT NULL
)
''')
conn.commit()

def add_student():
    roll = input("Enter Roll No: ")
    name = input("Enter Name: ")
    marks = int(input("Enter Marks: "))
    cursor.execute("INSERT INTO students VALUES (?, ?, ?)", (roll, name, marks))
    conn.commit()
    print("Student added successfully!\n")

def view_students():
    cursor.execute("SELECT * FROM students")
    rows = cursor.fetchall()
    print("\n--- Student List ---")
    for row in rows:
        print(f"Roll No: {row[0]}, Name: {row[1]}, Marks: {row[2]}")
    print()

def search_student():
    roll = input("Enter Roll No to search: ")
    cursor.execute("SELECT * FROM students WHERE roll_no=?", (roll,))
    row = cursor.fetchone()
    if row:
        print(f"Found: Roll No: {row[0]}, Name: {row[1]}, Marks: {row[2]}\n")
    else:
        print("Student not found!\n")

def delete_student():
    roll = input("Enter Roll No to delete: ")
    cursor.execute("DELETE FROM students WHERE roll_no=?", (roll,))
    conn.commit()
    print("Student deleted if existed.\n")

while True:
    print("1. Add Student")
    print("2. View All Students")
    print("3. Search Student")
    print("4. Delete Student")
    print("5. Exit")
    choice = input("Enter choice: ")
    if choice == '1':
        add_student()
    elif choice == '2':
        view_students()
    elif choice == '3':
        search_student()
    elif choice == '4':
        delete_student()
    elif choice == '5':
        break
    else:
        print("Invalid choice! Try again.\n")
