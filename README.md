# Semester_Project_FoCP
## MCQs Quiz App

### Team  
| Name          | CMS ID | Role                                      |
|---------------|--------|-------------------------------------------|
| Vishal Kumar  | 544454 | 1. Logic designer  2. Programmer          |
| Mohsin Abbas  | 540316 | 1. Programmer        2. Git Manager       |

---

## Overview

The goal of this project is to build a fully functional, console-based MCQs Quiz Application that can be used by a teacher (Admin) and students (Users). The program allows the admin to create topics, add MCQs with difficulty levels, manage students, and view their quiz records, while users can log in, take timed quizzes, and view their own results.

This project is a practical application of basic file handling, arrays, loops, and time handling in C++, and it shows how to build a menu-driven system with separate Admin and User flows.

---

## Program Structure

The whole logic is implemented in a single C++ file (for example `main.cpp`) and uses several functions to separate concerns.

Main functions:  
- `main()` – prints the welcome message and calls `ChooseMode` to select Admin or User.  
- `ChooseMode(...)` – asks the user to choose Admin, User, or Exit and then routes them to the correct mode.  

Admin-related functions:  
- `AdminMode(...)` – main menu for admin operations.  
- `AddNewTopic(...)` – adds a new topic and MCQs for a selected difficulty.  
- `RemoveTopic()` – removes topic files for a specific difficulty or removes the whole topic.  
- `AddMcqsToTopic(...)` – adds more MCQs to existing topic files.  
- `ViewTopicMcqs(...)` – displays all MCQs of a selected topic and difficulty.  
- `AddStudents()` – adds new student usernames to the system.  
- `ViewStudentsRecord()` – shows the stored quiz records of a selected student.  

User-related functions:  
- `UserMode(...)` – handles login and user menu.  
- `MakeQuiz(...)` – runs the full quiz process, including timer, scoring, result saving, and solution analysis.  
- `ViewRecords(...)` – shows the quiz history for the current user.  

Utility function:  
- `getHiddenInput()` – reads password from keyboard without showing characters, using `_getch()` from `<conio.h>`.  

---

## Data and Files

The application uses simple text files for storage so that data stays even after the program exits.

- **topicNames.txt**  
  - Contains the list of all topics added by the admin, one topic name per line.  

- **Usernames.txt**  
  - Contains all student usernames that are allowed to use User Mode.  

- **<TopicName>Easy.txt, <TopicName>Medium.txt, <TopicName>Hard.txt**  
  - For each topic, there can be up to three files, one per difficulty level.  
  - When a new topic and MCQs are added, the program creates or appends to these files.  
  - The first line of each file stores the difficulty name (for example `Easy`).  
  - After that, each MCQ is stored in 7 lines:
    1. `Q: <question>`  
    2. `A) <option A>`  
    3. `B) <option B>`  
    4. `C) <option C>`  
    5. `D) <option D>`  
    6. `<correct option>` (e.g. `A`, `B`, `C`, or `D`)  
    7. `-----------------` (separator line)

- **<username>.txt**  
  - For each student, a personal record file is created.  
  - It stores topic names, scores, and time taken for each quiz attempt.  

- **wrong.txt**  
  - Temporary file used during a quiz to save which questions were answered wrong and what option the user chose.  
  - At the end of the analysis, this file is cleared.  

---

## Execution Instructions

This section explains how the program behaves from start to end and how the user interacts with it.

### Start Screen and Mode Selection

When you run the program, it first prints a colored welcome banner saying "WELCOME TO MCQS QUIZ APP".  
Then it shows a menu:  
1. Admin  
2. User  
3. Exit APP  

You must enter `1`, `2`, or `3`. If you enter something else, it prints "Invalid Command!" and asks again.

- If you enter `3`, the program prints "YOU EXIT THE APP" and closes.  
- If you enter `1`, the program asks for the admin password using hidden input.  
- If you enter `2`, the program enters User Mode.  

---

### Admin Mode Flow

To enter Admin Mode, you choose option `1` from the main menu and then enter the password.  
The hardcoded admin password is `vishal16`. If the password is wrong, the program prints "Wrong password!" and returns to the mode selection menu.

Once logged in, Admin Mode works as follows:  
- The program first asks:  
  - `1` to continue Admin mode  
  - `2` to exit Admin mode  
- If you continue, it shows the Admin menu:

1. Add Topic  
2. Remove Topic  
3. Add MCQs to a Topic  
4. View Topics  
5. Add Students  
6. View Students records  
7. Exit Admin Mode  

If you enter any other number, it prints "Invalid Command!" and asks again.

#### 1. Add Topic

- The program repeatedly asks:  
  - `1. continue, enter new topic`  
  - `2. Back to Admin`  
- If you select continue:
  - It asks for a topic name (e.g. `Math`, `C++`, `Physics`).  
  - It checks if this topic already exists in `topicNames.txt`; if not, it appends it.  
  - Then it asks for difficulty:  
    1. Easy  
    2. Medium  
    3. Hard  
  - Depending on the choice, it opens `<TopicName>Easy.txt`, `<TopicName>Medium.txt`, or `<TopicName>Hard.txt` and, if the file is empty, writes the difficulty name on the first line.  
  - Then it enters a loop to add MCQs:
    - Asks for question text.  
    - Asks for options A, B, C, D.  
    - Asks for the correct option.  
    - Writes all this to the topic file in the specified 7-line format.  
    - Asks if you want to:
      - `1. continue, entering mcqs`  
      - `2. Back`  
    - If you choose `1`, you add the next question; if `2`, you stop and go back.  

#### 2. Remove Topic

- The program first shows all topics from `topicNames.txt`.  
- Then it asks you to type the topic name you want to remove.  
- If the topic is not found, it prints an error and asks again.  
- If found, you can choose:
  1. Easy  
  2. Medium  
  3. Hard  
  4. Whole topic  
- For options 1–3, it builds a filename like `<TopicName>Easy.txt` and asks for confirmation (`1. Yes  2. No`). If confirmed and file exists, it deletes that file.  
- For option 4, it tries to delete all three difficulty files and then removes the topic name from `topicNames.txt` using a temporary file.  

#### 3. Add MCQs to a Topic

- Shows the list of all topics.  
- Asks you to enter a topic name from the list.  
- If valid, asks for difficulty (Easy/Medium/Hard).  
- Opens the corresponding file (creates header if empty) and then repeatedly asks for:
  - question, options A–D, and correct option.  
- Each new question is appended in the same 7-line format, until you choose to go back.  

#### 4. View Topics (MCQs)

- Shows the list of topics and asks which topic you want to view.  
- Then asks for difficulty (Easy, Medium, Hard).  
- Opens the correct file and prints all lines.  
- Every 7th line is treated as the correct option and is printed with "Correct option Is = " before it.  

#### 5. Add Students

- Repeatedly lets you:
  - Press `1` to continue adding students.  
  - Press `2` to go back to Admin menu.  
- Before adding a new username, it prints the list of already added students from `Usernames.txt`.  
- If the username is new, it appends it to `Usernames.txt` and prints a confirmation message.  

#### 6. View Students Records

- Prints all usernames from `Usernames.txt`.  
- Asks for the username you want to view.  
- If the username exists, it calls `ViewRecords(username)`, which prints all entries from `<username>.txt` with some formatting.  

#### 7. Exit Admin Mode

- Returns back to the main mode selection screen.  

---

### User Mode Flow

To use User Mode, choose option `2` from the main menu.  
The flow is:  
- The program asks:
  - `1. continue User mode`  
  - `2. Exit`  
- If you continue, it asks you to enter a username.  
- It checks if this username is present in `Usernames.txt`; if not, it prints "Enter the correct username!" and asks again.  
- On success, it prints a welcome banner with your username and opens your record file (`<username>.txt`) for appending.  

Then the User menu is shown:  
1. Take Quiz  
2. View Records  
3. Exit User Mode  

If you press:  
- `2` – it calls `ViewRecords(username)` to display all your previous results.  
- `3` – it exits User Mode and returns to the main menu.  
- `1` – it starts the quiz selection process.  

#### Taking a Quiz

Steps to take a quiz:  
1. The program shows all topics from `topicNames.txt`.  
2. You type the topic name you want to attempt.  
3. If the topic is valid, it asks for difficulty:
   - `1. Easy`  
   - `2. Medium`  
   - `3. Hard`  
4. It builds the filename like `<TopicName>Easy.txt` or `<TopicName>Medium.txt` etc. and checks if this file exists.  
5. If the file is not present, it shows a message and asks you to pick again.  
6. If it exists, it calls `MakeQuiz(TopicNameWithDifficulty, username, difficulty)`.  

---

### MakeQuiz: Quiz, Timer, Results and Analysis

Inside `MakeQuiz(...)`, the complete quiz logic works as follows:

1. It opens the selected topic file (e.g. `MathEasy.txt`) and counts how many lines it contains to calculate how many MCQs are available.  
   - Because each MCQ uses 7 lines, it divides the count by 7 to find `AvaQues`.  

2. It asks the user:  
   - "Enter the number of MCQS for QUIZ!" and tells the maximum available.  
   - If the number is invalid (≤ 0 or greater than available questions), it shows an error and asks again.  

3. Once a valid number `Noofmcqs` is entered, it explains:  
   - You have `Noofmcqs` minutes to attempt the quiz (i.e. N MCQs in N minutes).  
   - The quiz cannot be paused or exited once started.  
   - Then asks:  
     - `1. Start Quiz`  
     - `2. EXIT Quiz`  

4. If you start the quiz:
   - It prints "QUIZ STARTS – BEST OF LUCK".  
   - It starts a timer using `chrono::high_resolution_clock`.  
   - For each question from 1 to `Noofmcqs`:
     - It checks the elapsed time; if total time ≥ `Noofmcqs * 60` seconds, it prints "Time is over!" and stops.  
     - For the very first question, it skips the first line (difficulty header) from the file.  
     - Then it reads and prints:
       - question line  
       - four option lines (A, B, C, D)  
     - It reads the stored correct option line.  
     - It asks: "Enter your Choice(A/B/C/D): " and reads your answer.  
     - It again checks if time is over; if yes, it stops the quiz early.  
     - If your answer matches the correct option, your score increases by 1.  
       Otherwise, the program writes the question number and your wrong answer into `wrong.txt`.  
     - Then it reads and discards the separator line and continues to the next question.  

5. After finishing questions or running out of time:
   - It calculates the percentage as `(score / Noofmcqs) * 100`.  
   - It calculates the total time taken in minutes (with one decimal place).  
   - It prints a "QUIZ RESULTS" block:
     - Score (e.g. "Score: 7 out of 10")  
     - Percentage, colored red if ≤ 50, otherwise green.  
     - Time taken in minutes.  
   - It writes a summary into `<username>.txt`, mentioning the topic, score, and time taken.  

6. Then it asks if you want a complete solution and analysis:
   - If you enter `1`, it shows a detailed review of each question.  
   - To do this, it:
     - Reads all wrong question numbers and wrong answers from `wrong.txt` into arrays.  
     - Rewinds the topic file to the beginning and again reads all questions in order.  
     - For each question:
       - Prints the question and all four options.  
       - Checks if this question number is in the wrong list.  
       - If it was wrong, it prints:
         - "Your Answer: <wrong option>" in red.  
         - "Correct Answer: <correct option>" in green.  
       - If it was correct, it only prints the correct answer.  
       - Prints a separator line.  
   - After printing full analysis, it clears `wrong.txt` by overwriting it with an empty file.  
   - Then it returns to User Mode.  

---

## Technical Notes

- The program uses `<conio.h>` and `_getch()` to hide password input for the admin. This works on Windows; for other platforms, this part may need changes.  
- ANSI escape codes are used for colored output (e.g. `\033[31m` for red), so the terminal must support them.  
- All data is stored as plain text files in the working directory; deleting these files will reset topics, users, or records.  
- The program uses `#include <chrono>` for high-resolution timing of quizzes.  
- File I/O uses `ifstream` and `ofstream` for reading and writing data.  

---

## How to Compile and Run

1. Make sure you have a C++ compiler that supports C++11 or later (e.g., g++, clang++, MSVC).  
2. Save the source code as `main.cpp`.  
3. Compile using:
   ```bash
   g++ -std=c++11 main.cpp -o mcqs_quiz_app
   ```
4. Run the program:
   ```bash
   ./mcqs_quiz_app
   ```

---

## Summary

This MCQs Quiz App is a complete, menu-driven console application suitable for classroom use. Teachers can quickly set up topics and questions, and students can take timed quizzes and review their performance. All data is automatically saved to text files, making the system persistent and easy to understand.
