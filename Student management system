#include <iostream>
#include <fstream>
#include <cstring>

using namespace std;

class Student {
public:
    int id;
    char name[50];
    int age;
    char course[50];

    void input() {
        cout << "Enter Student ID: ";
        cin >> id;
        cin.ignore();

        cout << "Enter Name: ";
        cin.getline(name, 50);

        cout << "Enter Age: ";
        cin >> age;
        cin.ignore();

        cout << "Enter Course: ";
        cin.getline(course, 50);
    }

    void display() {
        cout << "\nID      : " << id;
        cout << "\nName    : " << name;
        cout << "\nAge     : " << age;
        cout << "\nCourse  : " << course;
        cout << "\n------------------------";
    }
};

// Add Student
void addStudent() {
    Student s;
    ofstream file("students.dat", ios::binary | ios::app);

    s.input();
    file.write((char*)&s, sizeof(s));

    file.close();
    cout << "\nStudent Record Added Successfully!\n";
}

// Display Students
void displayStudents() {
    Student s;
    ifstream file("students.dat", ios::binary);

    if (!file) {
        cout << "\nNo Records Found!\n";
        return;
    }

    cout << "\n===== Student Records =====\n";

    while (file.read((char*)&s, sizeof(s))) {
        s.display();
    }

    file.close();
}

// Search Student
void searchStudent() {
    Student s;
    int searchId;
    bool found = false;

    cout << "Enter Student ID to Search: ";
    cin >> searchId;

    ifstream file("students.dat", ios::binary);

    while (file.read((char*)&s, sizeof(s))) {
        if (s.id == searchId) {
            cout << "\nStudent Found:\n";
            s.display();
            found = true;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nStudent Not Found!\n";
}

// Update Student
void updateStudent() {
    Student s;
    int searchId;
    bool found = false;

    cout << "Enter Student ID to Update: ";
    cin >> searchId;

    fstream file("students.dat", ios::binary | ios::in | ios::out);

    while (file.read((char*)&s, sizeof(s))) {
        if (s.id == searchId) {
            cout << "\nCurrent Details:\n";
            s.display();

            cout << "\nEnter New Details:\n";
            s.input();

            int pos = file.tellg();
            file.seekp(pos - sizeof(s));

            file.write((char*)&s, sizeof(s));

            found = true;
            cout << "\nRecord Updated Successfully!\n";
            break;
        }
    }

    file.close();

    if (!found)
        cout << "\nStudent Not Found!\n";
}

// Delete Student
void deleteStudent() {
    Student s;
    int searchId;
    bool found = false;

    cout << "Enter Student ID to Delete: ";
    cin >> searchId;

    ifstream file("students.dat", ios::binary);
    ofstream temp("temp.dat", ios::binary);

    while (file.read((char*)&s, sizeof(s))) {
        if (s.id == searchId) {
            found = true;
        } else {
            temp.write((char*)&s, sizeof(s));
        }
    }

    file.close();
    temp.close();

    remove("students.dat");
    rename("temp.dat", "students.dat");

    if (found)
        cout << "\nRecord Deleted Successfully!\n";
    else
        cout << "\nStudent Not Found!\n";
}

// Main Menu
int main() {
    int choice;

    do {
        cout << "\n\n===== STUDENT MANAGEMENT SYSTEM =====";
        cout << "\n1. Add Student";
        cout << "\n2. Display Students";
        cout << "\n3. Search Student";
        cout << "\n4. Update Student";
        cout << "\n5. Delete Student";
        cout << "\n6. Exit";
        cout << "\nEnter Choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addStudent();
                break;

            case 2:
                displayStudents();
                break;

            case 3:
                searchStudent();
                break;

            case 4:
                updateStudent();
                break;

            case 5:
                deleteStudent();
                break;

            case 6:
                cout << "\nExiting Program...\n";
                break;

            default:
                cout << "\nInvalid Choice! Try Again.\n";
        }

    } while (choice != 6);

    return 0;
}