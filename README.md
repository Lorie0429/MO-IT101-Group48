/*
============================================================
             MO-IT101-Group48
       CP1-MS2 MotorPH Payroll System
============================================================

1. Project Description
The MotorPH Payroll System is a Java-based program that reads employee
information and attendance records from CSV files and calculates the total
hours worked per payroll cutoff period.

The system processes attendance logs and summarizes work hours according to
MotorPH’s payroll schedule:

- Cutoff 1: Day 1–15
- Cutoff 2: Day 16–End of Month

This milestone focuses on file processing, attendance computation, and
payroll summary display.


2. System Objectives
The program aims to:

1) Read employee information from a CSV file.
2) Retrieve attendance records for a specific employee.
3) Compute daily working hours using login and logout times.
4) Group total hours based on payroll cutoff periods.
5) Display a payroll summary for each cutoff.


3. System Architecture

3.1 Data Source
The system reads two CSV files:

File                          Description
---------------------------------------------------------
Employee Details CSV          Contains employee personal information
Attendance Record CSV         Contains daily login and logout times


3.2 Project Directory Structure

MotorPH-Payroll-System
│
├── resources
│   ├── MotorPH_Employee Data - Employee Details.csv
│   └── MotorPH_Employee Data - Attendance Record.csv
│
├── src
│   └── MotorPH.java
│
└── README.md


4. Technologies Used

Technology        Purpose
-------------------------------------------
Java              Main programming language
CSV Files         Data storage for employee and attendance records
Java Time API     Handling login/logout times
BufferedReader    Reading CSV files
Scanner           User input handling


5. Java Libraries Used

import java.io.BufferedReader;
import java.io.FileReader;
import java.time.Duration;
import java.time.LocalTime;
import java.time.YearMonth;
import java.time.format.DateTimeFormatter;
import java.util.Scanner;


Explanation

Library             Purpose
------------------------------------------------
BufferedReader      Reads file line by line
FileReader          Opens CSV file
LocalTime           Handles time values
Duration            Calculates time difference
YearMonth           Gets number of days in a month
DateTimeFormatter   Formats time strings
Scanner             Accepts user input


6. Program Execution Flow

Step 1 – User Input

The program requests the employee number.

Enter Employee #:

This value will be used to search the employee details file.


Step 2 – Load Employee Details

The system reads the employee CSV file and searches for the entered employee number.

Processing steps:

Open employee details CSV file.
Skip the header row.
Read each line.
Split data using commas.
Compare employee number.

If the employee exists, the following details are stored:

Employee Number
Last Name
First Name
Birthday

If the employee does not exist, the system prints:

Employee does not exist.


7. Display Employee Information

Once the employee record is found, the program prints:

===================================
Employee # :
Employee Name :
Birthday :
===================================

This confirms the selected employee before processing attendance.


8. Attendance Processing

The program processes attendance records from:

June – December 2024

For each month, two counters are used:

Variable      Description
---------------------------------------------
firstHalf     Total hours from day 1–15
secondHalf    Total hours from day 16–end

The last day of each month is calculated using:

YearMonth.of(2024, month).lengthOfMonth();


9. Reading Attendance Records

The system reads the attendance CSV file line by line.

Processing steps:

Skip the header row.
Ignore empty rows.
Split the row using commas.
Process only rows that match the selected employee number.


10. Date and Time Processing

Each attendance record contains:

Field       Description
-------------------------------
Date        Attendance date
Log In      Employee login time
Log Out     Employee logout time

Example values:

06/03/2024
8:59
18:31

Time values are parsed using:

DateTimeFormatter.ofPattern("H:mm");


11. Computing Daily Work Hours

The system calculates hours worked using the method:

computeHours(LocalTime login, LocalTime logout)


Work Rules Applied

Rule                 Value
---------------------------------
Grace Period         8:10 AM
Work End Time        5:00 PM
Lunch Break          1 Hour
Maximum Work Hours   8 Hours


Logic

If logout exceeds 5:00 PM, it is adjusted to 5:00 PM.
Total minutes worked are calculated.
60 minutes are deducted for lunch.
Minutes are converted to hours.
Hours are limited to maximum of 8 hours.
If login is before 8:10 AM, the employee receives the full 8 hours credit.


12. Payroll Cutoff Assignment

Hours worked are grouped based on the day of the month.

Day Range     Payroll Cutoff
----------------------------
1–15          First Cutoff
16–End        Second Cutoff


13. Payroll Summary Output

For each month, the program prints the summary.

First Cutoff (1–15)

Total Hours Worked
Gross Salary
Net Salary


Second Cutoff (16–End)

Total Hours Worked
Gross Salary

Deductions
SSS
PhilHealth
Pag-IBIG
Tax

Net Salary

Salary and deduction values are currently placeholders.


14. Current System Limitations

The system only processes attendance records from 2024.
Salary and deduction calculations are not yet implemented.
CSV files must remain inside the resources folder.


15. Future Improvements

The following features may be added in future milestones:

Salary computation based on hourly rate
Automatic calculation of deductions (SSS, PhilHealth, Pag-IBIG, Tax)
Support for multiple payroll years
Export payroll results to a file
Graphical user interface (GUI)

============================================================
*/

public class MotorPH_ProjectDoc {

}PH_ProjectDoc {
    // Documentation container class
}
