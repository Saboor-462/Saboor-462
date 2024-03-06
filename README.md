#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Student {
    string name;
    int id;
    double gpa;
};

class DynamicArray {
private:
    Student* arr;
    int size;
    int capacity;

public:
    DynamicArray(int capacity) : size(0), capacity(capacity) {
        arr = new Student[capacity];
    }

    ~DynamicArray() {
        delete[] arr;
    }

    void addStudent(const Student& student) {
        if (size < capacity) {
            arr[size++] = student;
            cout << "Student added successfully." << endl;
        } else {
            cout << "Dynamic array is full. Cannot add more students." << endl;
        }
    }

    int searchStudent(int id) {
        for (int i = 0; i < size; ++i) {
            if (arr[i].id == id) {
                return i;
            }
        }
        return -1; // Student not found
    }

    void deleteStudent(int id) {
        int index = searchStudent(id);
        if (index != -1) {
            for (int i = index; i < size - 1; ++i) {
                arr[i] = arr[i + 1];
            }
            size--;
            cout << "Student deleted successfully." << endl;
        } else {
            cout << "Student not found." << endl;
        }
    }

    void saveToFile(const string& filename) {
        ofstream file(filename);
        if (file.is_open()) {
            for (int i = 0; i < size; ++i) {
                file << arr[i].name << endl;
                file << arr[i].id << endl;
                file << arr[i].gpa << endl;
            }
            cout << "Student data saved to " << filename << endl;
            file.close();
        } else {
            cout << "Unable to open file " << filename << endl;
        }
    }

    void loadFromFile(const string& filename) {
        ifstream file(filename);
        if (file.is_open()) {
            size = 0;
            while (!file.eof()) {
                getline(file, arr[size].name);
                file >> arr[size].id;
                file >> arr[size].gpa;
                file.ignore(); // ignore newline
                if (!file.eof()) {
                    size++;
                }
            }
            cout << "Student data loaded from " << filename << endl;
            file.close();
        } else {
            cout << "Unable to open file " << filename << endl;
        }
    }

    void displayStudents() {
        if (size == 0) {
            cout << "Dynamic array is empty." << endl;
            return;
        }

        cout << "Students in the dynamic array:" << endl;
        for (int i = 0; i < size; ++i) {
            cout << "Name: " << arr[i].name << endl;
            cout << "ID: " << arr[i].id << endl;
            cout << "GPA: " << arr[i].gpa << endl << endl;
        }
    }
};

DynamicArray dynamicArray(10);

void addStudent() {
    Student newStudent;
    cout << "Enter student details:" << endl;
    cout << "Name: ";
    cin.ignore();
    getline(cin, newStudent.name);
    cout << "ID: ";
    cin >> newStudent.id;
    cout << "GPA: ";
    cin >> newStudent.gpa;

    dynamicArray.addStudent(newStudent);
}

void searchStudent() {
    int id;
    cout << "Enter student ID to search: ";
    cin >> id;
    int index = dynamicArray.searchStudent(id);
    if (index != -1) {
        cout << "Student found at index " << index << endl;
    } else {
        cout << "Student not found." << endl;
    }
}

void deleteStudent() {
    int id;
    cout << "Enter student ID to delete: ";
    cin >> id;
    dynamicArray.deleteStudent(id);
}

void saveToFile() {
    string filename;
    cout << "Enter filename to save: ";
    cin >> filename;
    dynamicArray.saveToFile(filename);
}

void loadFromFile() {
    string filename;
    cout << "Enter filename to load: ";
    cin >> filename;
    dynamicArray.loadFromFile(filename);
}

void displayStudents() {
    dynamicArray.displayStudents();
}

int main() {
    while (true) {
        cout << "Dynamic Array Student Management System" << endl;
        cout << "1. Add a student" << endl;
        cout << "2. Search for a student" << endl;
        cout << "3. Delete a student" << endl;
        cout << "4. Save student data to file" << endl;
        cout << "5. Load student data from file" << endl;
        cout << "6. Display all students" << endl;
        cout << "7. Exit" << endl;
        cout << "Enter your choice: ";

        int choice;
        cin >> choice;

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                searchStudent();
                break;
            case 3:
                deleteStudent();
                break;
            case 4:
                saveToFile();
                break;
            case 5:
                loadFromFile();
                break;
            case 6:
                displayStudents();
                break;
            case 7:
                cout << "Exiting program." << endl;
                return 0;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    }

    return 0;
}
