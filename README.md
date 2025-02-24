# Hairston_DatabaseFileAssignment
A company wants to create a database to store employee id, first name, last name and total income. They decided to give 5 characters for id, 15 characters for first name, 15 characters for last name and 10 characters for income. The table below summarizes the character limits.
Id (5 chars)	First name (15 chars)	Last name (15 chars)	Income (10 chars)

In addition, all employers’ id starts with ‘E’ followed by a number, like ‘E0001’ or ‘E0005’
Income: is a float value representing the total income for that employee. It takes 10 numbers at maximum.
The following table summarizes a sample of the data stored in the file:
Id	First name	Last name	Income
5 chars	15 chars	15 chars	10 numbers
E0001	Dawn	Dixon	100000.00
E0002	Mike	Jonathan	56000.25
E0003	Erin	Brown	75000.00

The data is stored in a database file called ‘data.txt’
We want to write a program in C++ to perform three operations: add a new employee, get employee information, and list all employees.
Complete the code below, make sure it runs correctly. Create a github repository called database, make sure to make the repository public. Submit your code to github repository database. Copy the link and paste it on Canvas.
The first thing we need to do is to include libraries:
#include <iostream>
#include <fstream>
#include <string>
using namespace std;
Declare variables for accessing the file and filename:
fstream file;
string filename = "data.txt";
Declare a struct to save the data:
struct Person {
    string id, firstName, lastName;
    int income;
};
The first function we are going to write is ‘search’, search takes an employee id and search for that employee in the file, if it finds it will return a pointer to the structure ‘Person’ otherwise return null.
struct Person *search(string id) {
	struct Person *p2 = new Person;
	char empId[6], firstName[15], lastName[15], incomeStr[10];
	file.clear();
	file.seekg(0, ios::beg);
	while (file.read(empId, 5)) {
		empId[5] = '\0';
		p2->id = empId;
        		if (p2->id == id) {
        	// Reading rest of data
        	file.read(firstName, 15); // read 15 chars
        	file.read(lastName, 15); // read 15 chars
        	file.read(incomeStr, 10); // read 10 chars
        	// Save to structure Person
        	p2->firstName = firstName;
        	p2->lastName = lastName;
        	p2->income = stof(incomeStr); // convert incomeStr to float
        	return p2; // return pointer to Person struct
		}
		file.seekg(40, ios::cur); // why 40?
    	}
    	return 0;
} 
Note: the line in bold where file.seekg uses 40 bytes from current file position. Explain why it is 40?

The next function is get an employee information from the employee id, the function get_employee ask the user to enter the employee id and call the function search and display output.
void get_employee() {
	string empId;
	// Ask the user to enter the employee id to get its information, complete code below
	

	// Then call the function search with the employee id as a parameter
	// Remember the search function returns a pointer to the struct ‘Person’
	// complete code below
	
	// Check if the pointer returned is null, then print employee not found
	// otherwise display pointer information.
	
}
The function list will list all employees’ information:
void list() {
	char id[6], firstName[15], lastName[15], incomeStr[10];
	file.clear();
	file.seekg(0, ios::beg); // rewind file to begining
	while (file.read(id, 5)) {
		// read rest of data
		file.read(firstName, 15);
		file.read(lastName, 15);
		file.read(incomeStr, 10);
		// save data to Person struct
		float income = atof(incomeStr); // convert string to float
		// display Person struct (record)
		cout << id << " " << firstName << " " << lastName << " " << income << endl;
    }
}
The function write_data will store data into the file:
void write_data(Person p) {
	// write data to file:
	file.clear();
	file.write(p.id.c_str(), 5); // write 5 chars for id
	file.write(p.firstName.c_str(), 15); // write 15 chars for first name
	file.write(p.lastName.c_str(), 15); // write 15 chars for last name
	file.write(to_string(p.income).c_str(), 10); // write 10 chars after converting to string
	file.flush();
}
The function new_employee, ask the user about a new employee data and call write_data to save the data into the file:
void new_employee() {
	string empId;
	while (1) {
		cout << "\n*** New Employee ***\n";
		cout << "Enter employee id (-1 to end): ";
		cin >> empId;
		if (empId == "-1") break; // stop when user enters -1
		// search if the employee exist, call to search function
		// Remember search function return a pointer to struct ‘Person’ if the employee exist
		// otherwise, it returns null pointer (i.e. zero pointer)
		// If the employee id exist, then exit from the function by calling return.
		// If employee id does not exist, then continue ask the user about the employee
		// information: first name, last name, and income.
		// complete code 
		
		// Save data to Person struct
		Person p;
		p.id = empId;
		cout << "Enter first name: ";
		cin >> p.firstName;
		cout << "Enter last name: ";
		cin >> p.lastName;
		cout << "Enter income: ";
		cin >> p.income;
		cout << "Saving data: ";
		write_data(p);
	}
}
Finally, we have the main:
int main() {
    file.open(filename, ios::in | ios::out); // open file for read and write
	if (!file) {
		file.open(filename, ios::out); // create the file
		file.close();
		file.open(filename, ios::in | ios::out);
	}
	// Menu:
    int option = -1;
    while (1) {
    	cout << "*** Menu ***" << endl;
    	cout << "1) New employee\n";
    	cout << "2) Search employee information\n";
    	cout << "3) List employee\n";
    	cout << "option: ";
    	cin >> option;
    	switch (option) {
    		case 1: new_employee(); break;
    		case 2: get_employee(); break;
    		case 3: list(); break;
    		default: // any other number will exit the menu loop
    			return 0;
		}
	}
	// Close files
	file.close();
    return 0;
}




