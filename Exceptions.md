C++ Exceptions
When executing C++ code, different errors can occur: coding errors made by the programmer, errors due to wrong input, or other unforeseeable things.

When an error occurs, C++ will normally stop and generate an error message. The technical term for this is: C++ will throw an exception (throw an error).

C++ try and catch
Exception handling in C++ consist of three keywords: try, throw and catch:

The try statement allows you to define a block of code to be tested for errors while it is being executed.

The throw keyword throws an exception when a problem is detected, which lets us create a custom error.

The catch statement allows you to define a block of code to be executed, if an error occurs in the try block.

The try and catch keywords come in pairs:

Example
try {
  // Block of code to try
  throw exception; // Throw an exception when a problem arise
}
catch () {
  // Block of code to handle errors
}
Consider the following example:

Example
try {
  int age = 15;
  if (age >= 18) {
    cout << "Access granted - you are old enough.";
  } else {
    throw (age);
  }
}
catch (int myNum) {
  cout << "Access denied - You must be at least 18 years old.\n";
  cout << "Age is: " << myNum;
}
Try it Yourself »
Example explained
We use the try block to test some code: If the age variable is less than 18, we will throw an exception, and handle it in our catch block.

In the catch block, we catch the error and do something about it. The catch statement takes a parameter: in our example we use an int variable (myNum) (because we are throwing an exception of int type in the try block (age)), to output the value of age.

If no error occurs (e.g. if age is 20 instead of 15, meaning it will be be greater than 18), the catch block is skipped:

Example
int age = 20;
You can also use the throw keyword to output a reference number, like a custom error number/code for organizing purposes (505 in our example):

Example
try {
  int age = 15;
  if (age >= 18) {
    cout << "Access granted - you are old enough.";
  } else {
    throw 505;
  }
}
catch (int myNum) {
  cout << "Access denied - You must be at least 18 years old.\n";
  cout << "Error number: " << myNum;
}
Try it Yourself »
Handle Any Type of Exceptions (...)
If you do not know the throw type used in the try block, you can use the "three dots" syntax (...) inside the catch block, which will handle any type of exception:

Example
try {
  int age = 15;
  if (age >= 18) {
    cout << "Access granted - you are old enough.";
  } else {
    throw 505;
  }
}
catch (...) {
  cout << "Access denied - You must be at least 18 years old.\n";
}
Date and Time
The <ctime> library allows us to work with dates and times.

To use it, you must import the <ctime> header file:

Example
#include <ctime> // Import the ctime library
Display Current Date and Time
The <ctime> library has a variety of functions to measure dates and times.

The time() function gives us a timestamp representing the current date and time. We can use the ctime() function to show the date and time that a timestamp represents:

Example
Display the current date:

// Get the timestamp for the current date and time
time_t timestamp;
time(&timestamp);

// Display the date and time represented by the timestamp
cout << ctime(&timestamp);
Try it Yourself »
Two ways to use the time() function
The time() function writes a timestamp to the memory location given by the parameter, but it also returns the timestamp's value.

An alternative way to use the time() function is to pass in a NULL pointer and use the return value instead.

time_t timestamp = time(NULL);
Data Types
There are two different data types used to store the date and time: time_t for timestamps and struct tm for datetime structures.

Timestamps represent a moment in time as a single number, which makes it easier for the computer to do calculations.

Datetime structures are structures that represent different components of the date and time as members. This makes it easier for us to specify dates. Datetime structures have the following members:

tm_sec - The seconds within a minute
tm_min - The minutes within an hour
tm_hour - The hour within a day (from 0 to 23)
tm_mday - The day of the month
tm_mon - The month (from 0 to 11 starting with January)
tm_year - The number of years since 1900
tm_wday - The weekday (from 0 to 6 starting with Sunday)
tm_yday - The day of the year (from 0 to 365 with 0 being January 1)
tm_isdst - Positive when daylight saving time is in effect, zero when not in effect and negative when unknown
Always keep in mind the way that date components are represented:

Hours are represented in 24-hour format. 11pm would be represented as 23.
The months go from 0 to 11. For example, December would be represented as 11 rather than 12.
Years are represented relative to the year 1900. The year 2024 would be represented as 124 because 124 years have passed since 1900.
Creating Timestamps
The time() function can only create a timestamp for the current date, but we can create a timestamp for any date by using the mktime() function.

The mktime() function converts a datetime structure into a timestamp.

Example
Create a timestamp using the mktime() function:

struct tm datetime;
time_t timestamp;

datetime.tm_year = 2023 - 1900; // Number of years since 1900
datetime.tm_mon = 12 - 1; // Number of months since January
datetime.tm_mday = 17;
datetime.tm_hour = 12;
datetime.tm_min = 30;
datetime.tm_sec = 1;
// Daylight Savings must be specified
// -1 uses the computer's timezone setting
datetime.tm_isdst = -1;

timestamp = mktime(&datetime);

cout << ctime(&timestamp);
Try it Yourself »
Note: The mktime() function needs these members to have a value: tm_year, tm_mon, tm_mday, tm_hour, tm_min, tm_sec and tm_isdst.

Creating Datetime Structures
The mktime() function also fills in the tm_wday and tm_yday members of the datetime structure with the correct values, which completes the structure and gives a valid datetime. It can be used, for example, to find the weekday of a given date:

Example
Find the weekday of a specified date:

// Create the datetime structure and use mktime to fill in the missing members
struct tm datetime;
datetime.tm_year = 2023 - 1900; // Number of years since 1900
datetime.tm_mon = 12 - 1; // Number of months since January
datetime.tm_mday = 17;
datetime.tm_hour = 0; datetime.tm_min = 0; datetime.tm_sec = 0;
datetime.tm_isdst = -1;
mktime(&datetime);

string weekdays[] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};

cout << "The date is on a " << weekdays[datetime.tm_wday];
Try it Yourself »
The localtime() and gmtime() functions can convert timestamps into datetime structures.

The localtime() function returns a pointer to a structure representing the time in the computer's time zone.

The gmtime() function returns a pointer to a structure representing the time in the GMT time zone.

These functions return a pointer to a datetime structure. If we want to make sure its value does not change unexpectedly we should make a copy of it by dereferencing the pointer. To learn about dereferencing, see the C++ Dereference tutorial.

Example
Get a datetime structure and output the current hour:

time_t timestamp = time(&timestamp);
struct tm datetime = *localtime(&timestamp);

cout << datetime.tm_hour;
Try it Yourself »
Display Dates
So far we have been using the ctime() function to display the date contained in a timestamp. To display dates from a datetime structure we can use the asctime() function.

Example
Display the date represented by a datetime structure:

time_t timestamp = time(NULL);
struct tm datetime = *localtime(&timestamp);

cout << asctime(&datetime);
Try it Yourself »
Note: The asctime() function does not correct invalid dates. For example, if you set the day of the month to 32 it will display 32. The mktime() function can correct these kinds of errors:

Example
Correct a date before displaying it:

// Create the datetime structure and use mktime to correct mistakes
struct tm datetime;
datetime.tm_year = 2022 - 1900; // Number of years since 1900
datetime.tm_mon = 0; // 0 is January
datetime.tm_mday = 32;
datetime.tm_hour = 0; datetime.tm_min = 0; datetime.tm_sec = 0;
datetime.tm_isdst = -1;
mktime(&datetime);

cout << asctime(&datetime);
Try it Yourself »
The ctime() and asctime() functions allow us to display the date but they do not allow us to choose how it is displayed.

To choose how a date is displayed we can use the strftime() function.

Example
Represent the current date in different ways:

time_t timestamp = time(NULL);
struct tm datetime = *localtime(&timestamp);

char output[50];

strftime(output, 50, "%B %e, %Y", &datetime);
cout << output << "\n";

strftime(output, 50, "%I:%M:%S %p", &datetime);
cout << output << "\n";

strftime(output, 50, "%m/%d/%y", &datetime);
cout << output << "\n";

strftime(output, 50, "%a %b %e %H:%M:%S %Y", &datetime);
cout << output << "\n";
Try it Yourself »
The strftime() function formats a date and writes it as a C-style string into a char array. It has four parameters:

The first parameter points to the char array where the formatted date will be written.
The second parameter specifies the space available in the array.
The third parameter allows us to choose how the date is formatted using format specifiers.
The last parameter is a pointer to the datetime structure which contains the date we want to display.
The following table has some useful format specifiers. For a more complete list, look at the strftime() reference page.
