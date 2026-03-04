# MO-IT101-Group48
CP1-MS2 MotorPH Payroll System 
Basic Payroll Program

This program reads employee information and attendance records from CSV files, calculates the total hours worked per payroll cutoff, and displays a simple salary summary.

How the Program Works
Imports
import java.io.BufferedReader;
import java.io.FileReader;
import java.time.Duration;
import java.time.LocalTime;
import java.time.YearMonth;
import java.time.format.DateTimeFormatter;
import java.util.Scanner;

BufferedReader and FileReader are used to read CSV files line by line.

LocalTime and Duration are used to handle time values and compute hours worked.

YearMonth helps determine the number of days in a month.

DateTimeFormatter formats the time values from the CSV file.

Scanner is used to get input from the user.

Main Class and Method
public class MotorPH {
    public static void main(String[] args) {

MotorPH is the main class of the program.

The main method is the starting point where the program begins execution.

File Paths and Scanner
String empFile = "resources/MotorPH_Employee Data - Employee Details.csv";
String attFile = "resources/MotorPH_Employee Data - Attendance Record.csv";
Scanner sc = new Scanner(System.in);

empFile and attFile store the file paths of the employee and attendance CSV files.

Scanner sc is used to read input from the keyboard.

Get Employee Number
System.out.print("Enter Employee #: ");
String inputEmpNo = sc.nextLine();

The program asks the user to enter an employee number.

The entered value will be used to search for the employee in the CSV file.

Prepare Variables
String empNo = "";
String firstName = "";
String lastName = "";
String birthday = "";
boolean found = false;

These variables store employee information once the record is found.

found is used to check whether the employee exists in the file.

Read Employee Details
try (BufferedReader br = new BufferedReader(new FileReader(empFile))) {
    br.readLine(); // Skip header
    String line;

    while ((line = br.readLine()) != null) {
        if (line.trim().isEmpty()) continue;

        String[] data = line.split(",");

        if (data[0].equals(inputEmpNo)) {
            empNo = data[0];
            lastName = data[1];
            firstName = data[2];
            birthday = data[3];
            found = true;
            break;
        }
    }
} catch (Exception e) {
    System.out.println("Error reading employee file.");
    return;
}

The program reads the employee CSV file line by line.

The header row is skipped.

Each row is split using commas to separate the fields.

The employee number is compared with the user input.

If a match is found, the employee information is stored.

Check if Employee Exists
if (!found) {
    System.out.println("Employee does not exist.");
    return;
}

If the employee number is not found in the file, the program stops.

Display Employee Information
System.out.println("\n===================================");
System.out.println("Employee # : " + empNo);
System.out.println("Employee Name : " + lastName + ", " + firstName);
System.out.println("Birthday : " + birthday);
System.out.println("===================================");

The program displays the employee number, full name, and birthday.

Prepare Time Format
DateTimeFormatter timeFormat = DateTimeFormatter.ofPattern("H:mm");

This tells Java how to read the login and logout time values from the CSV file.

Process Attendance by Month
for (int month = 6; month <= 12; month++) {
    double firstHalf = 0;
    double secondHalf = 0;
    int daysInMonth = YearMonth.of(2024, month).lengthOfMonth();

The program loops from June to December 2024.

firstHalf stores hours worked from day 1 to 15.

secondHalf stores hours worked from day 16 to the end of the month.

daysInMonth determines the total days in the month.

Read Attendance File
try (BufferedReader br = new BufferedReader(new FileReader(attFile))) {
    br.readLine(); // Skip header
    String line;

    while ((line = br.readLine()) != null) {
        if (line.trim().isEmpty()) continue;

        String[] data = line.split(",");

        if (!data[0].equals(empNo)) continue;

The program reads the attendance CSV file line by line.

Empty rows are skipped.

Only records that match the selected employee number are processed.

Parse Date and Time
String[] dateParts = data[3].split("/");
int recordMonth = Integer.parseInt(dateParts[0]);
int day = Integer.parseInt(dateParts[1]);
int year = Integer.parseInt(dateParts[2]);

if (year != 2024 || recordMonth != month) continue;

LocalTime login = LocalTime.parse(data[4].trim(), timeFormat);
LocalTime logout = LocalTime.parse(data[5].trim(), timeFormat);

double hours = computeHours(login, logout);

The date is split into month, day, and year.

Only records from the correct month and year (2024) are processed.

Login and logout times are parsed.

The computeHours() method calculates the hours worked.

Add Hours to Payroll Cutoffs
if (day <= 15)
    firstHalf += hours;
else
    secondHalf += hours;

If the day is 1–15, hours are added to the first payroll cutoff.

If the day is 16–end of month, hours are added to the second cutoff.

Display Payroll Summary
System.out.println("\nCutoff Date: " + monthName + " 1 to 15");
System.out.println("Total Hours Worked : " + firstHalf);
System.out.println("Gross Salary: ");
System.out.println("Net Salary: ");

System.out.println("\nCutoff Date: " + monthName + " 16 to " + daysInMonth);
System.out.println("Total Hours Worked : " + secondHalf);
System.out.println("Gross Salary: ");
System.out.println("Deductions: ");
System.out.println(" SSS: ");
System.out.println(" PhilHealth: ");
System.out.println(" Pag-IBIG: ");
System.out.println(" Tax: ");
System.out.println("Net Salary: ");

The program displays the payroll cutoff dates and total hours worked.

Gross salary, deductions, and net salary are placeholders for future implementation.

Compute Hours Method
static double computeHours(LocalTime login, LocalTime logout) {

    LocalTime graceTime = LocalTime.of(8, 10);
    LocalTime cutoffTime = LocalTime.of(17, 0);

    if (logout.isAfter(cutoffTime)) {
        logout = cutoffTime;
    }

    long minutesWorked = Duration.between(login, logout).toMinutes();

    if (minutesWorked > 60) {
        minutesWorked -= 60; // Lunch break
    } else {
        minutesWorked = 0;
    }

    double hours = minutesWorked / 60.0;

    if (!login.isAfter(graceTime)) {
        return 8.0; // Full 8 hours if on time
    }

    return Math.min(hours, 8.0);
}

Sets the grace period (8:10 AM) and cutoff time (5:00 PM).

Limits logout time to 5:00 PM if later.

Deducts 1 hour for lunch if the employee worked more than one hour.

Converts minutes worked into hours.

Returns the total hours worked, capped at 8 hours per day.

Notes

CSV files must be located inside the resources folder.

The program currently shows placeholders for salary and deductions, which can be implemented later.

The system processes attendance records for year 2024 only.

Payroll cutoffs are 1–15 and 16–end of month.
