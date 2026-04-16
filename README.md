# CPP-Practicals


### Use Case : Student Record System with Classes and Objects Scenario: You need to develop a student record management system for a college. The system should allow the addition of student records, modification, and display of records, as well as calculations like average grades. Tasks: Class Design: Create a Student class with data members like roll_number, name, and marks. Implement a constructor to initialize student records when an object is created and a destructor to clean up resources if needed. Include methods like addStudent(), modifyStudent(), displayStudent(), and calculateAverage(). Use overloaded constructors to initialize students with different data (e.g., full info vs just roll number). Approach: Class and objects: Use a class to represent the student, encapsulating the details of each student. Inheritance: You could extend this to have subclasses like GraduateStudent or UndergraduateStudent, which inherit from Student and add more specialized data. Polymorphism: Use method overloading for various operations like addStudent() (with different types of arguments).

```
#include <iostream>
#include <vector>
using namespace std;

class Student {
private:
    int roll;
    string name;
    float marks[3];

public:
    // Default constructor
    Student() : roll(0), name(""), marks{0,0,0} {}

    // Parameterized constructor
    Student(int r, string n, float m[3]) {
        roll = r; name = n;
        for(int i = 0; i < 3; i++) marks[i] = m[i];
    }

    // Constructor with just roll number
    Student(int r) : roll(r), name("Unknown"), marks{0,0,0} {}

    ~Student() {}

    void modify(string n, float m[3]) {
        name = n;
        for(int i = 0; i < 3; i++) marks[i] = m[i];
    }

    float calculateAverage() {
        return (marks[0] + marks[1] + marks[2]) / 3.0;
    }

    void display() {
        cout << "Roll: " << roll << " | Name: " << name
             << " | Avg: " << calculateAverage() << endl;
    }
};

// Graduate student inherits Student
class GraduateStudent : public Student {
    string thesis;
public:
    GraduateStudent(int r, string n, float m[3], string t)
        : Student(r, n, m), thesis(t) {}

    void display() {
        Student::display();
        cout << "  Thesis: " << thesis << endl;
    }
};

int main() {
    float m1[] = {85, 90, 78};
    float m2[] = {88, 92, 95};

    Student s1(101, "Raj", m1);
    Student s2(102);                        

    float mod[] = {70, 75, 80};
    s2.modify("Priya", mod);

    GraduateStudent g1(201, "Aryan", m2, "AI in Healthcare");

    cout << "--- Student Records ---\n";
    s1.display();
    s2.display();
    g1.display();
    return 0;
}
```
<img width="513" height="186" alt="image" src="https://github.com/user-attachments/assets/c5e54fb4-8d2f-4e97-a47e-6eb8711f8ce9" />


### Use Case : Employee Salary Management System Using File Handling Scenario: You need to build an employee salary management system that reads employee records from a file, calculates their salary, and writes the updated data back to the file. Tasks: Create an Employee class with attributes like employee_id, name, and salary. Implement a method calculateSalary() which calculates salary based on some business logic (e.g., bonuses, deductions). Use file handling to: Read employee data from a text file (ifstream). Write updated employee data back to the file (ofstream). Implement a main function that loads employee records from the file, calculates their salary, and saves the updated records back to the file. Approach: File Handling: Use ifstream to read the employee data and ofstream to write the data back after processing. Object-Oriented Design: Use a class to represent employees and encapsulate their data. Exception Handling: Implement error checking to ensure the file exists and is accessible.

```
#include <iostream>
#include <fstream>
#include <vector>
#include <stdexcept>
using namespace std;

class Employee {
public:
    int id;
    string name;
    float baseSalary;

    Employee(int i, string n, float s) : id(i), name(n), baseSalary(s) {}

    float calculateSalary() {
        float bonus     = baseSalary * 0.10;
        float deduction = baseSalary * 0.05;
        return baseSalary + bonus - deduction;
    }

    void display() {
        cout << "ID: " << id << " | Name: " << name
             << " | Base: " << baseSalary
             << " | Net: "  << calculateSalary() << endl;
    }
};

void saveToFile(vector<Employee>& emps) {
    ofstream fout("employees.txt");
    if (!fout) throw runtime_error("Cannot open file for writing!");
    for (auto& e : emps)
        fout << e.id << " " << e.name << " " << e.baseSalary << "\n";
    fout.close();
}

vector<Employee> loadFromFile() {
    ifstream fin("employees.txt");
    if (!fin) throw runtime_error("File not found!");
    vector<Employee> emps;
    int id; string name; float sal;
    while (fin >> id >> name >> sal)
        emps.push_back(Employee(id, name, sal));
    fin.close();
    return emps;
}

int main() {
    try {
        int n;
        cout << "How many employees do you want to add? ";
        cin >> n;

        vector<Employee> emps;

        for (int i = 0; i < n; i++) {
            int id; string name; float sal;
            cout << "\n--- Employee " << i+1 << " ---\n";
            cout << "Enter ID     : "; cin >> id;
            cout << "Enter Name   : "; cin >> name;
            cout << "Enter Salary : "; cin >> sal;
            emps.push_back(Employee(id, name, sal));
        }

        saveToFile(emps);
        cout << "\nRecords saved!\n";

        // Reload from file and display
        vector<Employee> loaded = loadFromFile();
        cout << "\n--- Salary Report ---\n";
        for (auto& e : loaded) e.display();

    } catch (exception& e) {
        cout << "Error: " << e.what() << endl;
    }
    return 0;
}
```


<img width="558" height="522" alt="image" src="https://github.com/user-attachments/assets/7f48497b-313f-4d7a-8bf4-c21c305cf911" />

### Use Case: Tic-Tac-Toe Game with Classes and Object-Oriented Design Scenario: Develop a Tic-Tac-Toe game using object-oriented design principles. The game should allow two players to take turns, check for a winner, and reset the game. Tasks: Create a Game class with a board (a 3x3 matrix), turn (to keep track of whose turn it is), and winner (to determine the winner). Implement methods for: resetGame(): Resets the board. makeMove(): Places a mark on the board. checkWinner(): Checks if there is a winner. printBoard(): Displays the current state of the board. Implement game logic to alternate between players and check if a player wins or if it’s a draw. Approach: Classes/Objects: Use a class to encapsulate game-related data and methods. Encapsulation: Keep game data (board, winner, turn) private inside the class. Polymorphism: You could also extend this with different game modes (e.g., player vs AI). 

```
#include <iostream>
using namespace std;

class Game {
private:
    char board[3][3];
    char turn;

public:
    string player1, player2;

    Game(string p1, string p2) : player1(p1), player2(p2) {
        resetGame();
    }

    void resetGame() {
        int k = 1;
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                board[i][j] = '0' + k++;
        turn = 'X';
    }

    void printBoard() {
        cout << "\n";
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++)
                cout << " " << board[i][j] << " ";
            cout << "\n";
        }
        cout << "\n";
    }

    bool makeMove(int pos) {
        if (pos < 1 || pos > 9) return false;
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                if (board[i][j] == '0' + pos) {
                    board[i][j] = turn;
                    turn = (turn == 'X') ? 'O' : 'X';
                    return true;
                }
        return false;   // cell already taken
    }

    char checkWinner() {
        for (int i = 0; i < 3; i++) {
            if (board[i][0]==board[i][1] && board[i][1]==board[i][2]) return board[i][0];
            if (board[0][i]==board[1][i] && board[1][i]==board[2][i]) return board[0][i];
        }
        if (board[0][0]==board[1][1] && board[1][1]==board[2][2]) return board[0][0];
        if (board[0][2]==board[1][1] && board[1][1]==board[2][0]) return board[0][2];
        return ' ';
    }

    bool isDraw() {
        for (int i = 0; i < 3; i++)
            for (int j = 0; j < 3; j++)
                if (board[i][j] != 'X' && board[i][j] != 'O')
                    return false;
        return true;
    }

    void play() {
        int pos;
        while (true) {
            printBoard();

            // Show whose turn it is by name
            string currentPlayer = (turn == 'X') ? player1 : player2;
            cout << currentPlayer << " (" << turn << ") — enter position (1-9): ";
            cin >> pos;

            if (!makeMove(pos)) {
                cout << "Invalid move! Try again.\n";
                continue;
            }

            char winner = checkWinner();
            if (winner != ' ') {
                printBoard();
                string winName = (winner == 'X') ? player1 : player2;
                cout << "🎉 " << winName << " wins!\n";
                return;
            }
            if (isDraw()) {
                printBoard();
                cout << "It's a draw!\n";
                return;
            }
        }
    }
};

int main() {
    string p1, p2;
    cout << "Enter Player 1 name (X): "; cin >> p1;
    cout << "Enter Player 2 name (O): "; cin >> p2;

    char choice;
    do {
        Game g(p1, p2);
        g.play();
        cout << "\nPlay again? (y/n): ";
        cin >> choice;
    } while (choice == 'y' || choice == 'Y');

    cout << "Thanks for playing!\n";
    return 0;
}
```

<img width="596" height="682" alt="image" src="https://github.com/user-attachments/assets/7d203581-1ee7-4556-b2b0-58b460611dc1" />
<img width="487" height="349" alt="image" src="https://github.com/user-attachments/assets/0a7a2f9b-6e98-413b-a74b-1ba8bbfa7ac6" />


