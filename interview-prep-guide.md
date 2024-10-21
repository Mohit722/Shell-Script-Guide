# Shell Scripting Basics:
----------------------------

1. What is a Shell?
  - A shell is an environment where we run Linux commands.

2. What is a Shell Script?
  - A shell script is a text file that contains a sequence of commands and logic for execution.

---

# Writing a Shell Script

When writing a shell script, the first line should specify the type of shell you want to use, like this:
```bash
#!/bin/bash
```
This is called a shebang and tells the system to use the Bash shell to execute the script.

---

# Executing a Shell Script

Before running a shell script, you need to make sure the file has execute permissions.

1. Give Execute Permission:
   ```bash
   chmod +x basic.sh
   ```
   This command gives the `basic.sh` script execute permission.

2. Run the Script:
   If you are in the same folder as the script, you can run it like this:
   ```bash
   ./basic.sh
   ```


# Running with Absolute Path
You can also run the script using its absolute path if you are not in the same folder:
```bash
/home/ubuntu/basic.sh
```

---


# Setting the PATH Variable

To run your script from anywhere (without giving the full path every time), you can add the script’s folder to your system’s PATH.

1. Check Current PATH:
   ```bash
   echo $PATH
   ```

2. Add Your Script's Location to PATH:
   ```bash
   export PATH=$PATH:/home/ubuntu
   ```

Now, you can run `basic.sh` from any folder without needing the full path:
```bash
basic.sh
```

---


# Running Shell Scripts Without Execute Permission

Even if you haven't given execute permission to a script, you can still run it using bash or sh:

- Without Shebang (`#!/bin/bash`):
  ```bash
  sh basic.sh
  bash basic.sh
  ```

- Removing Execute Permission:
  If you remove execute permission:
  ```bash
  chmod -x basic.sh
  ```
  Running the script like this will give you "Permission Denied":
  ```bash
  ./basic.sh
  ```
  But you can still run it using bash or sh:
  ```bash
  bash basic.sh
  ```

This is useful in scenarios where you are automating tasks like downloading a script via `git clone`, and the script does not have execute permission. Instead of adding permission using `chmod +x`, you can directly run it with `bash` or `sh`.





# Understanding Variables in Shell Scripting with Real-Time Scenarios:
--------------------------------------------------------------------------

Variables are a crucial part of shell scripting. They store data that can be reused and manipulated as your script runs. This flexibility allows for automation, dynamic behavior, and better control over the script's execution.

# What is a Variable?

A variable is essentially a named storage location in the system’s memory. It holds a value that can be accessed or modified during the script’s execution. This makes variables useful when you want to reuse data or capture the result of a command and use it later.

Variables come into play when:
- You have a value you need to reuse multiple times.
- You want to dynamically store information during script execution.
- You need to capture the result of one command and use it in the next.

---


# Defining and Using Variables

# Defining a Variable

In shell scripting, you define a variable by assigning a value to it. No data type is specified explicitly (like in other programming languages), and variables can hold numbers, strings, or even commands.

Syntax:
```bash
variable_name=value
```

Examples:

```bash
name="John Doe"
age=30
```

Here, `name` stores a string and `age` stores a number. Quotes are necessary for strings, but not for numeric values.



# Retrieving the Value from a Variable

To access the value stored in a variable, use the `$` symbol followed by the variable’s name.

```bash
echo $name
echo $age
```

This will output:
```
John Doe
30
```

---


# Real-Time Scenario: Using Variables for Reusability

Let’s assume you are writing a script to automate a task that involves connecting to multiple servers. Instead of hardcoding the server name every time, you can store it in a variable.

```bash
#!/bin/bash
server_name="prod-server-01"

echo "Connecting to $server_name..."
ssh user@$server_name
```

Now, if you need to connect to a different server, you only need to update the variable `server_name`. The rest of the script remains unchanged, making it more maintainable.

---


# Types of Variables

# 1. Local Variables
These are the default variables in shell scripts. They exist only in the script or function where they are defined. Once the script ends, the variable is no longer available.

Example:
```bash
#!/bin/bash
greeting="Hello"
echo $greeting
```

When this script ends, `greeting` is no longer available.



# 2. Environment Variables
These are global variables that are available to the script as well as the shell from which the script is run. Environment variables are passed from the parent shell to child shells.

To make a variable an environment variable, you use the `export` command.

```bash
export var_name=value
```

Example:
```bash
export USER_NAME="John"
```

Now, `USER_NAME` is available not only in the current script but also in any other scripts or child processes started from this shell.

---


# Real-Time Scenario: Using Environment Variables

Imagine you have multiple scripts that rely on a common variable, such as an API key. Instead of defining the API key in every script, you can define it as an environment variable in the shell. This allows all scripts to access the same value.

```bash
export API_KEY="abc123"

./script1.sh   # Can access API_KEY
./script2.sh   # Can also access API_KEY
```

In `script1.sh` and `script2.sh`, you can use:
```bash
echo "The API key is: $API_KEY"
```

This way, if the API key changes, you only need to update the environment variable once, and all scripts will use the new value.

---

# Example: Overwriting Variable Values in a Script

When you define a variable inside a script, it can be updated or overwritten multiple times as the script runs.

```bash
#!/bin/bash
counter=10
echo "Initial value of counter: $counter"

counter=20
echo "Updated value of counter: $counter"
```

Output:
```
Initial value of counter: 10
Updated value of counter: 20
```

---


# Passing Variables Between Shells

1. Parent to Child Shell:
   - Environment variables defined in the parent shell can be passed to child processes or scripts.
   - For example, if you export a variable in the terminal, any script you run will have access to it.

```bash
# In the terminal
export MYNAME="Alice"
./child_script.sh
```

In `child_script.sh`:
```bash
#!/bin/bash
echo "Your name is: $MYNAME"
```

This will output:
```
Your name is: Alice
```

2. Child to Parent Shell:
   - Variables defined in a script (child shell) are not available to the parent shell by default.

Example:
```bash
#!/bin/bash
MYORG="MyCompany"
echo "Organization: $MYORG"
```
When this script finishes, `MYORG` won’t be accessible from the terminal.

---


# Running a Script in the Same Shell

If you want a script to run in the same shell (without creating a new one), you can use the dot (`.`) or source command. This is useful when you want the changes made in the script (like setting variables) to persist after the script finishes.

```bash
# source command
source ./script.sh

# or dot notation
. ./script.sh
```


Real-Time Scenario:

Let’s say you have a script that sets up environment variables for your project. You want those variables to be available after the script runs.

```bash
#!/bin/bash
export DATABASE_URL="jdbc:mysql://localhost:3306/mydb"
```

Run this script with `source` or `.`:
```bash
source ./set_env.sh
```

Now, `DATABASE_URL` will be available in your current shell session.

---

# Conclusion

Variables are powerful tools in shell scripting that allow you to create dynamic, reusable, and maintainable scripts. Whether you're working with local or environment variables, understanding how to define, retrieve, and manipulate them is key to creating effective scripts. Real-time scenarios like connecting to servers, storing API keys, or setting up environment variables show how variables can simplify tasks and improve script automation.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Special Variables in Shell Scripting

In shell scripting, there are several special variables that are built into the shell environment. These special variables provide useful information like the script's filename, command-line arguments, exit status, and process ID. Let's dive into them with examples and real-time usage.

---

### List of Special Variables

| Variable | Description                                                  |
|--------------|------------------------------------------------------------------|
| `$0`         | Returns the filename of the script.                              |
| `$n`         | Command-line arguments, where `n` is a number from 1 to 9.       |
| `$#`         | Returns the total number of arguments passed to the script.      |
| `$?`         | Returns the exit status of the last command executed.            |
| `$$`         | Returns the process ID (PID) of the current script.              |
| `shift`      | Reduces the number of arguments each time it executes.           |

---

# Example of Using Special Variables

Assume we have a script called `var.sh` and you pass two arguments to it from the command line:

```bash
# ./var.sh value1 value2
```

This will return:
```bash
echo $0    # var.sh (the script's filename)
echo $1    # value1 (first argument)
echo $2    # value2 (second argument)
```

You can pass up to 9 command-line arguments using `$1` to `$9`.

---


# Example Script: `special_vars.sh`

Here’s a script that demonstrates various special variables:

```bash
#!/bin/bash

echo "File name \$0: $0"
echo "Total number of arguments passed \$#: $#"
echo "All the arguments \$@ : $@"

echo "--- -- --"
echo "First argument \$1 : $1"
echo "Second argument \$2 : $2"
echo "Third argument \$3 : $3"
echo "Fourth argument \$4 : $4"
echo "Fifth argument \$5 : $5"
echo "Sixth argument \$6 : $6"
echo "Seventh argument \$7 : $7"
echo "Eighth argument \$8 : $8"
echo "Ninth argument \$9 : $9"

echo "-- -- --"
echo "Exit status of the previous command \$? : $?"
echo "Process ID of the current script \$$ : $$"
```


# How to Run It

```bash
# bash special_vars.sh 1 2 3 4 5
```

This will print:

```
File name $0: special_vars.sh
Total number of arguments passed $# : 5
All the arguments $@ : 1 2 3 4 5

--- -- --
First argument $1 : 1
Second argument $2 : 2
Third argument $3 : 3
Fourth argument $4 : 4
Fifth argument $5 : 5
Sixth argument $6 :
Seventh argument $7 :
Eighth argument $8 :
Ninth argument $9 :

-- -- --
Exit status of the previous command $? : 0
Process ID of the current script $$ : 12345  # Example PID
```

---


# Explanation of Key Special Variables:

1. `$0`: Displays the script’s filename, which is useful when you want to know the script being executed.

2. `$1`, `$2`, ... `$9`: Represents the positional parameters (arguments) passed to the script. You can access them with `$n`, where `n` is the argument number.

   Example:
   ```bash
   ./script.sh arg1 arg2 arg3
   echo $1    # Outputs: arg1
   echo $2    # Outputs: arg2
   ```

3. `$#`: Shows the total number of arguments passed to the script.

4. `$?`: Returns the exit status of the last executed command. This is useful to check if the last command executed successfully (`0` for success, non-zero for failure).

   Example:
   ```bash
   ls /nonexistent_directory
   echo $?   # Outputs: 2 (since the directory doesn't exist)
   ```

5. `$$`: Displays the process ID (PID) of the current script, useful for debugging or when you need to track the script's process.

6. `shift`: Shifts positional parameters to the left. For example, after calling `shift`, `$2` becomes `$1`, `$3` becomes `$2`, and so on.

---

# Real-Time Scenario: Exporting and Escaping Variables

Let’s say you want to define an environment variable, `MYORG`, and use it within a script:

```bash
#!/bin/bash
export MYORG="WEZVATECH"
echo "Your organization is \$MYORG : $MYORG"
```

Run the script:
```bash
# ./var.sh
```

This will output:
```
Your organization is $MYORG : WEZVATECH
```

If you want to print `$MYORG` literally without resolving the variable, use the escape character `\`:
```bash
echo "Your organization is \$MYORG"
```

This will print:
```
Your organization is $MYORG
```

---


# Checking Exit Status and Process ID

After running a command, you can check the exit status using `$?`. For example:
```bash
ls /some_directory
echo $?   # Outputs: 0 (if successful), non-zero if there's an error
```

To know the process ID of the current script:
```bash
echo $$   # Outputs: process ID of the running script
```

---

# Conclusion

Special variables in shell scripting offer powerful ways to handle command-line arguments, check command statuses, and retrieve system-level information like process IDs. Using these variables can help you write more dynamic and flexible scripts, making them an essential part of shell scripting.




# Real-Time Use Cases for Special Variables in Shell Scripting

Special variables in shell scripting are often used in automation tasks, DevOps processes, or system administration where you need to handle command-line arguments, check script statuses, manage processes, or dynamically pass information between scripts. Here are some real-time use cases for these variables:

---

# 1. Automating Backup Scripts with Command-Line Arguments

In a real-world scenario, you might have a backup script that needs to accept multiple arguments like the source directory, destination directory, and the type of backup (full or incremental). 

Script: `backup.sh`

```bash
#!/bin/bash

if [ $# -lt 3 ]; then
  echo "Usage: $0 <source_dir> <destination_dir> <backup_type>"
  exit 1
fi

SOURCE_DIR=$1
DEST_DIR=$2
BACKUP_TYPE=$3

echo "Starting $BACKUP_TYPE backup from $SOURCE_DIR to $DEST_DIR"
# Here we can include the actual backup command such as rsync
rsync -a $SOURCE_DIR $DEST_DIR

echo "Backup completed!"
echo "Exit status: $?"
```

How to Run It:
```bash
# ./backup.sh /home/user/documents /mnt/backup_drive full
```

- `$0`: Gives the script name, useful for displaying usage messages.
- `$1`, `$2`, `$3`: Captures the source directory, destination directory, and backup type.
- `$#`: Checks if the correct number of arguments is passed.
- `$?`: After running the backup, checks if the command was successful.

---


# 2. Monitoring Process Execution in Jenkins Pipeline

Suppose you're writing a script to check the status of a process in a Jenkins pipeline or any CI/CD environment. You can use `$?` to validate if the previous command ran successfully and `$$` to monitor the process ID.

Script: `process_monitor.sh`

```bash
#!/bin/bash

echo "Starting a background task..."
sleep 60 &  # Simulate a long-running process
PROCESS_ID=$!

echo "Process ID of the background task: $$"
echo "Waiting for the process to complete..."

wait $PROCESS_ID

if [ $? -eq 0 ]; then
  echo "Process completed successfully!"
else
  echo "Process failed with exit code: $?"
fi
```

Use Case:
This script can be used in a CI/CD pipeline to monitor background processes (like build, test, or deployment). It will capture the process ID using `$$` and check if the process was successful using `$?`.

---


# 3. Handling Multiple User Inputs in Automation Scripts

Imagine you're writing a deployment script where you need to pass multiple arguments like server IP, username, and password. You can use the special variables to handle those arguments and perform tasks accordingly.

Script: `deploy.sh`

```bash
#!/bin/bash

if [ $# -lt 3 ]; then
  echo "Usage: $0 <server_ip> <username> <password>"
  exit 1
fi

SERVER_IP=$1
USERNAME=$2
PASSWORD=$3

echo "Deploying application to $SERVER_IP with user $USERNAME"

# Connect to the server using SSH and deploy
ssh $USERNAME@$SERVER_IP "echo 'Deploying app'; exit"
```

Run Command:
```bash
# ./deploy.sh 192.168.0.10 admin pass123
```

- `$0`: Prints the script name for usage clarification.
- `$1`, `$2`, `$3`: Captures the server IP, username, and password for deployment.
- `$#`: Ensures that the correct number of arguments is provided.

---

# 4. Data Processing Script for Multiple Files

You might write a script that processes multiple files, where each filename is passed as an argument. You can use `$#` to count the number of files and `$@` to loop through all the filenames.

Script: `process_files.sh`

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
  echo "No files provided for processing."
  exit 1
fi

echo "Processing $# files: $@"

for file in "$@"; do
  echo "Processing file: $file"
  # Process each file (e.g., compress or copy)
  gzip "$file"
done

echo "All files processed successfully."
```

Run Command:
```bash
# ./process_files.sh file1.txt file2.txt file3.txt
```

- `$@`: Contains all the arguments passed (i.e., all filenames).
- `$#`: Shows how many files (arguments) were provided.

---


# 5. Switching Context Between Parent and Child Scripts

In real-world automation, you might need to pass variables from one script (parent) to another (child). This is commonly used in multi-script workflows where environment variables are shared across scripts.

Parent Script: `parent.sh`

```bash
#!/bin/bash

export DEPLOY_ENV="production"
echo "Deploying in environment: $DEPLOY_ENV"

./child.sh
```

Child Script: `child.sh`

```bash
#!/bin/bash

echo "Received environment from parent: $DEPLOY_ENV"
```

How to Run:
```bash
# ./parent.sh
```

- `$DEPLOY_ENV`: Passed from the parent script to the child script.

---


# 6. Managing Multiple User Inputs in System Administration

System administrators often use special variables to manage scripts for tasks like user creation, where inputs are passed via command-line arguments.

Script: `create_user.sh`

```bash
#!/bin/bash

if [ $# -lt 3 ]; then
  echo "Usage: $0 <username> <password> <user_group>"
  exit 1
fi

USERNAME=$1
PASSWORD=$2
USER_GROUP=$3

echo "Creating user $USERNAME in group $USER_GROUP..."

# Add the user
useradd -m -G $USER_GROUP $USERNAME
echo $USERNAME:$PASSWORD | chpasswd

echo "User $USERNAME created successfully."
```

Run Command:
```bash
# ./create_user.sh john secretpassword developers
```

- `$1`, `$2`, `$3`: Captures the username, password, and user group for user creation.
- `$#`: Ensures the correct number of arguments are provided.

---


# 7. Dynamic Argument Handling with Shift

In more complex scripts, you might need to handle a dynamic number of arguments. The `shift` command allows you to process one argument at a time.

Script: `dynamic_args.sh`

```bash
#!/bin/bash

echo "Processing arguments..."

while [ $# -gt 0 ]; do
  echo "Argument: $1"
  shift
done
```

Run Command:
```bash
# ./dynamic_args.sh arg1 arg2 arg3 arg4
```

- `shift`: Each time the loop runs, `shift` removes the first argument and shifts the remaining ones to the left. This is useful for processing a variable number of inputs.

---

# Conclusion

Special variables in shell scripting are essential for handling command-line arguments, monitoring script execution, and managing processes. They play a crucial role in automation, system administration, and DevOps workflows, enabling you to write more dynamic, flexible, and efficient scripts that can adapt to various scenarios.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Shell Script Troubleshooting: 
--------------------------------

When you write a Linux shell script, it can contain many commands and complex logic for automation. Some scripts might be 50, 100, or even 200 lines long. Troubleshooting becomes challenging when you encounter an error because the shell script typically won't tell you exactly where the issue is occurring. It will simply try to run the commands and show a general error message.

To identify which line is causing the error in the script, you can use the `-vx` option, which stands for verbose (`v`) and debugging (`x`). This will show you the exact line being executed, making it easier to trace and fix errors.

#### Example:

Script without Debugging:

```bash
#!/bin/bash
export MYORG="WEZVATECH"
echo "YOUR NAME IS @MYORG : $MYORG"
```

Output without Debugging:
```bash
YOUR NAME IS @MYORG : WEZVATECH
```

If this script runs fine, you'll see the output in a single line. However, if there's an error in a large script, it's hard to know exactly where it is.

#### Using the `-vx` Option for Troubleshooting:

To make troubleshooting easier, you can add `set -vx` at the beginning of the script. This will show you each line as it is being executed, providing more information for debugging.

Script with Debugging:

```bash
#!/bin/bash
set -vx  # Enable verbose and debug mode

export MYORG="WEZVATECH"
echo "YOUR NAME IS @MYORG : $MYORG"
```

How It Helps:
- `v` (verbose): Shows the script commands before they are executed.
- `x` (debug): Shows each command and its expanded arguments as they are executed.

Output with Debugging:
```bash
+ export MYORG=WEZVATECH
+ echo 'YOUR NAME IS @MYORG : WEZVATECH'
YOUR NAME IS @MYORG : WEZVATECH
```

Here, the `+` symbol before each line shows what the script is doing step-by-step, helping you to identify exactly where things might go wrong.



# Real-Time Use Case: Debugging a Loop or Conditional Script
------------------------------------------------------------

Imagine you're writing a script with a loop or condition that processes multiple files. If the script fails, it can be difficult to know which part of the loop or condition caused the issue. Using `set -vx`, you can track exactly where the script is failing.

Script Example:
```bash
#!/bin/bash
set -vx

for file in file1 file2 file3; do
  echo "Processing $file"
  cp $file /destination
done
```

If the script fails to copy one of the files, `-vx` will show you the exact point where it failed, making it easier to debug and fix the problem.

# Conclusion:

Using the `-vx` option in your shell scripts is a powerful way to debug issues, especially when dealing with complex scripts involving loops, conditions, or many lines of commands. It gives you a detailed view of what each line is doing, helping you quickly identify and fix errors.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Shell Script Basic Operators:
---------------------------------

# What are Operators and Why Do We Need Them?
In a shell script, when you run commands or use variables to store values, you will often need to perform operations on those values. Operations could involve arithmetic, comparisons, or checking strings, and that’s where operators come in.

# What Do You Mean by Operations?
For example, if you're dealing with numbers, you may want to add, subtract, multiply, or divide them. To do this, you need to combine the values using an operator. Similarly, if you have two numbers and want to compare them (e.g., check if one is greater than the other), you need relational operators. The same goes for strings, where you might want to check if two strings are equal or if a string is empty.


# Types of Operators in Shell Scripts:
- Arithmetic Operators: Used for mathematical operations.
- Relational Operators: Used for comparisons between numbers.
- Boolean Operators: Used for logical operations.
- String Operators: Used to compare strings or check if a string is empty.
- File Test Operators: Used to check properties of files (e.g., whether a file exists or is readable).

---

# Arithmetic Operators in Shell:
---------------------------------

Shell uses an external program called `expr` for arithmetic operations because the shell itself doesn’t directly support arithmetic. You need two values and an operator to perform any arithmetic operation.

 Syntax:
```bash
expr value1 <operator> value2
```

# Arithmetic Operators:
- `+`: Addition
- `-`: Subtraction
- ``: Multiplication (use `\` in shell scripts to escape the symbol)
- `/`: Division
- `%`: Modulus (returns the remainder of a division)
- `=`: Assignment (assigns the right operand to the left)
- `==`: Equality (checks if two numbers are the same)
- `!=`: Not Equal (checks if two numbers are different)


# Example Script:

```bash
# nano arithmetic.sh

#!/bin/sh

var1=30
var2=20

echo "var1=$var1 var2=$var2"

add=`expr $var1 + $var2`
echo "var1 + var2 : $add"

sub=`expr $var1 - $var2`
echo "var1 - var2 : $sub"

mul=`expr $var1 \ $var2`
echo "var1  var2 : $mul"

div=`expr $var2 / $var1`
echo "var2 / var1 : $div"

mod=`expr $var2 % $var1`
echo "var2 % var1 : $mod"

var3=$var1
echo "var3 value : $var3"
```

#### Running the Script:
```bash
$ ./arithmetic.sh
```
The script will calculate the results of basic arithmetic operations (addition, subtraction, multiplication, etc.) between `var1` and `var2` and print them.

---


# Real-Time Use Case:

Problem: Calculate the Average of Three Numbers

Imagine you need a shell script that calculates the average of three values. You can add the three numbers and then divide the sum by 3 to get the average.

Example Script:

```bash
#!/bin/sh

var1=23
var2=45
var3=67

sum=`expr $var1 + $var2 + $var3`
avg=`expr $sum / 3`

echo "Average of $var1, $var2, $var3 is $avg"
```

Explanation:
- The script adds the three numbers (`var1`, `var2`, `var3`).
- The sum is then divided by 3 to get the average.
- The result is printed as the average.

This is a practical example where you might need to use arithmetic operators in shell scripts for calculations.





# Relational Operators in Shell Scripts:
-----------------------------------------

Relational operators are used in shell scripts to compare two numbers and determine if one is equal to, greater than, or less than the other. These comparisons are helpful when writing conditions in shell scripts, especially in automation tasks like those seen in DevOps.

#### Relational Operators and Their Descriptions:
- `-eq`: Checks if the values of two operands are equal. If yes, the condition is true.
- `-ne`: Checks if the values of two operands are not equal. If not equal, the condition is true.
- `-gt`: Checks if the value of the left operand is greater than the right operand.
- `-lt`: Checks if the value of the left operand is less than the right operand.
- `-ge`: Checks if the value of the left operand is greater than or equal to the right operand.
- `-le`: Checks if the value of the left operand is less than or equal to the right operand.

#### Example Syntax:
```bash
if [ value1 -eq value2 ]; then
    echo "Both are equal"
fi
```

---

# Real-Time Use Cases in DevOps:
--------------------------------

# 1. Monitoring Disk Usage:
You may want to check if disk usage has exceeded a certain limit on your server and take action accordingly.

```bash
#!/bin/bash
disk_usage=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

if [ $disk_usage -gt 80 ]; then
    echo "Disk usage is over 80%. Please clean up!"
else
    echo "Disk usage is under control."
fi
```
- Explanation: This script checks if the disk usage is greater than 80%. If it is, it will print a message indicating that cleanup is required.


# 2. Checking CPU Load:
In DevOps, you may want to monitor CPU usage and trigger alerts if CPU load exceeds a certain threshold.

```bash
#!/bin/bash
cpu_load=$(cat /proc/loadavg | awk '{print $1}')

if [ $(echo "$cpu_load > 1.5" | bc) -eq 1 ]; then
    echo "CPU load is high: $cpu_load"
else
    echo "CPU load is normal: $cpu_load"
fi
```
- Explanation: The script checks if the CPU load exceeds `1.5`. If the load is higher, it triggers an alert.


# 3. Validating Application Versions:
When deploying a new version of an application, you can compare the version numbers and ensure that the correct version is running on the server.

```bash
#!/bin/bash
installed_version="2.1.0"
required_version="2.1.0"

if [ "$installed_version" -eq "$required_version" ]; then
    echo "Application is up-to-date."
else
    echo "Please update the application to version $required_version."
fi
```
- Explanation: This script checks if the installed version of the application matches the required version.


# 4. Ensuring Minimum Memory Availability:
When deploying applications, you might want to check if there is sufficient free memory before proceeding with deployment.

```bash
#!/bin/bash
free_memory=$(free -m | grep Mem | awk '{print $4}')

if [ $free_memory -lt 1000 ]; then
    echo "Not enough memory. At least 1GB required."
else
    echo "Memory check passed. Proceeding with deployment."
fi
```
- Explanation: This script checks if there is less than 1GB of free memory. If there isn't enough memory, it stops the deployment.


# 5. Checking the Number of Running Containers (Docker):
You can check the number of running containers and take action if too many or too few are running.

```bash
#!/bin/bash
running_containers=$(docker ps -q | wc -l)

if [ $running_containers -lt 3 ]; then
    echo "Warning: Less than 3 containers are running."
else
    echo "Sufficient containers are running."
fi
```
- Explanation: The script checks if there are fewer than three running Docker containers and prints a warning if necessary.

---

# Using Relational Operators in a Script Example:
--------------------------------------------------

# Example Script:
```bash
#!/bin/bash

# Assigning values to variables
num1=50
num2=100

# Checking if the values are equal
if [ $num1 -eq $num2 ]; then
    echo "$num1 is equal to $num2"
else
    echo "$num1 is not equal to $num2"
fi

# Checking if num1 is greater than num2
if [ $num1 -gt $num2 ]; then
    echo "$num1 is greater than $num2"
else
    echo "$num1 is not greater than $num2"
fi

# Checking if num1 is less than or equal to num2
if [ $num1 -le $num2 ]; then
    echo "$num1 is less than or equal to $num2"
else
    echo "$num1 is greater than $num2"
fi
```

# Running the Script:
```bash
$ ./relational_operators.sh
```

# Output:
```bash
50 is not equal to 100
50 is not greater than 100
50 is less than or equal to 100
```

---

# Summary:
Relational operators in shell scripts are essential for comparisons, especially when you’re automating tasks in DevOps. Whether you are monitoring system resources, comparing application versions, or validating deployment conditions, these operators make your scripts smarter and more dynamic.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Shell Script Decision Making Made Simple:
---------------------------------------------

Decision-making in shell scripting helps you to run specific sections of code based on conditions. Here's an easy explanation of how it works, along with real-time examples.

# Key Concepts:
- Any non-zero or non-null value is considered TRUE.
- Negation of a TRUE value with `!` makes it FALSE.

---

# Example of Decision Flow:

```bash
#!/bin/bash

line1
if [ condition ]; then
   line2
   line3
fi
line4
line5
```

# Explanation:
- If condition is TRUE: The script will execute line2 and line3 and continue to line4 and line5.
- If condition is FALSE: The script skips line2 and line3, jumping directly from line1 to line4 and line5.

---

# `If` Statement

The `if` statement allows you to execute a block of code only when a certain condition is true.

 Syntax:
```bash
if [ condition ]; then
  # Code to run if the condition is true
fi
```

Example:

```bash
#!/bin/bash

var1=1
var2=0

# This block runs because $var1 is a non-zero value (True)
echo "-- TRUE Condition --"
if [ $var1 ]; then
  echo "1) Got a true expression value"
fi

# This block will not run because $var1 != $var2
echo -e "\n-- Check if values are equal --"
if [ $var1 == $var2 ]; then
   echo "2) var1 is equal to var2"
fi

# This block will run because $var1 != $var2
echo -e "\n-- Check if values are not equal --"
if [ $var1 != $var2 ]; then
   echo "3) var1 is not equal to var2"
fi
```


#### Output:
```bash
-- TRUE Condition --
1) Got a true expression value

-- Check if values are equal --

-- Check if values are not equal --
3) var1 is not equal to var2
```

---


# `If..Else` Statement
-------------------------

The `if..else` statement allows you to run one block of code if the condition is true, and another block of code if the condition is false.

#### Syntax:
```bash
if [ condition ]; then
  # Code to run if the condition is true
else
  # Code to run if the condition is false
fi
```

#### Example:
```bash
#!/bin/bash

a=10
b=20

if [ $a -eq $b ]; then
  echo "$a -eq $b: a is equal to b"
else
  echo "$a -eq $b: a is not equal to b"
fi

if [ $a -ne $b ]; then
  echo "$a -ne $b: a is not equal to b"
else
  echo "$a -ne $b: a is equal to b"
fi

if [ $a -gt $b ]; then
  echo "$a -gt $b: a is greater than b"
else
  echo "$a -gt $b: a is not greater than b"
fi

if [ $a -lt $b ]; then
  echo "$a -lt $b: a is less than b"
else
  echo "$a -lt $b: a is not less than b"
fi

if [ $a -ge $b ]; then
  echo "$a -ge $b: a is greater or equal to b"
else
  echo "$a -ge $b: a is not greater or equal to b"
fi

if [ $a -le $b ]; then
  echo "$a -le $b: a is less or equal to b"
else
  echo "$a -le $b: a is not less or equal to b"
fi
```

#### Output:
```bash
10 -eq 20: a is not equal to b
10 -ne 20: a is not equal to b
10 -gt 20: a is not greater than b
10 -lt 20: a is less than b
10 -ge 20: a is not greater or equal to b
10 -le 20: a is less or equal to b
```

---

# Real-Time Use Cases in DevOps:
-----------------------------------

# 1. Checking if a Service is Running:
Before deploying an application, you might check if a necessary service is running.

```bash
#!/bin/bash
service="nginx"
if systemctl is-active --quiet $service; then
  echo "$service is running"
else
  echo "$service is not running. Starting it now."
  sudo systemctl start $service
fi
```


# 2. Validating Available Disk Space:
You can check if your server has enough free space before performing a task like log archiving or software installation.

```bash
#!/bin/bash
free_space=$(df / | tail -1 | awk '{print $4}')
min_required_space=1000000

if [ $free_space -lt $min_required_space ]; then
  echo "Not enough free space for installation."
else
  echo "Proceeding with installation."
fi
```


# 3. Deployment Condition:
Check if the application version is the latest before deploying.

```bash
#!/bin/bash
current_version="2.0.1"
required_version="2.1.0"

if [ "$current_version" == "$required_version" ]; then
  echo "The application is up to date."
else
  echo "Deploying the latest version."
  # Code to update and deploy the new version
fi
```

---

# Conclusion:
The `if` and `if..else` statements in shell scripts are powerful tools for decision-making, especially in automating processes in DevOps. They allow you to check conditions and perform specific actions based on the outcome, making your scripts adaptable and efficient.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# String Operators in Shell Scripting
---------------------------------------

In shell scripting, string operators allow you to compare and check the values of strings, whether they are equal, not equal, empty, or have a certain size.

# Operators Overview:

| Operator | Description                                           |
|----------|-------------------------------------------------------|
| =        | Checks if the value of two strings are equal or not    |
| !=       | Checks if the value of two strings are not equal       |
| -z       | Checks if the given string is empty (zero length)      |
| -n       | Checks if the given string is non-empty (non-zero)     |
| str      | Checks if the string `str` is not empty                |

---

# Example Script with String Operators

```bash
#!/bin/sh

a="adam"
b="wezvatechnologies"

# Checking if the two strings are equal
if [ $a = $b ]
then
  echo "$a = $b : a is equal to b"
else
  echo "$a = $b : a is not equal to b"
fi

# Checking if the two strings are not equal
if [ $a != $b ]
then
  echo "$a != $b : a is not equal to b"
else
  echo "$a != $b : a is equal to b"
fi

# Checking if the string 'a' has zero length
if [ -z $a ]
then
  echo "-z $a : string length is zero"
else
  echo "-z $a : string length is not zero"
fi

# Checking if the string 'a' has a non-zero length
if [ -n $a ]
then
  echo "-n $a : string length is not zero"
else
  echo "-n $a : string length is zero"
fi

# Checking if the string 'a' is not empty
if [ $a ]
then
  echo "$a : string is not empty"
else
  echo "$a : string is empty"
fi
```


# Explanation:

1. `[ $a = $b ]`: Checks if the strings `a` and `b` are equal. In this case, since `"adam"` is not equal to `"wezvatechnologies"`, the condition will be false.
   
2. `[ $a != $b ]`: Checks if `a` and `b` are not equal. Since `"adam"` and `"wezvatechnologies"` are indeed not equal, this condition will be true.

3. `[ -z $a ]`: Checks if the length of string `a` is zero. Since `"adam"` has characters, this condition will be false.

4. `[ -n $a ]`: Checks if the length of string `a` is non-zero. Since `"adam"` has characters, this condition will be true.

5. `[ $a ]`: Checks if `a` is not an empty string. This condition is true because `a` contains `"adam"`.

---


# Real-Time Use Case in DevOps:
---------------------------------

#### 1. Checking for an empty configuration variable:
When running deployment scripts, you may want to ensure a certain environment variable or configuration file path is set.

```bash
#!/bin/bash
config_path="/etc/myapp/config.yml"

if [ -z "$config_path" ]; then
  echo "Config file path is missing. Exiting..."
  exit 1
else
  echo "Using config file at $config_path"
fi
```

# 2. Validating a version string:
You may want to check if a version string provided during deployment matches the expected version.

```bash
#!/bin/bash
expected_version="v1.0.0"
current_version="v1.2.0"

if [ $current_version != $expected_version ]; then
  echo "Current version $current_version is not equal to the expected version $expected_version."
else
  echo "Version check passed."
fi
```

---

# Conclusion:

String operators in shell scripting are essential when comparing or checking the properties of strings, such as equality, non-equality, and whether they are empty or non-empty. These checks are crucial in automating tasks like file paths, version control, and environment variable validation in DevOps workflows.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# File Test Operators in Shell Scripting
-------------------------------------------

In Linux shell scripting, file operators are used to check different properties of a file, such as its existence, permissions, size, and type. These operators help you make decisions in your script based on file properties.

### Commonly Used File Operators:

| Operator | Description                                      |
|----------|--------------------------------------------------|
| -r       | Checks if the file has read permission            |
| -w       | Checks if the file has write permission           |
| -x       | Checks if the file has execute permission         |
| -f       | Checks if the file is a regular (ordinary) file   |
| -d       | Checks if the file is a directory                 |
| -s       | Checks if the file size is greater than zero      |
| -e       | Checks if the file exists                        |

---

# Example Script Using File Operators:

```bash
#!/bin/sh

file="/home/mohit/Adam-devops/shell-script/conditions/wexvatech.txt"

# Check if the file has read access
if [ -r $file ]
then
  echo "File has read access"
else
  echo "File does not have read access"
fi

# Check if the file has write permission
if [ -w $file ]
then
  echo "File has write permission"
else
  echo "File does not have write permission"
fi

# Check if the file has execute permission
if [ -x $file ]
then
  echo "File has execute permission"
else
  echo "File does not have execute permission"
fi

# Check if the file is a regular file (not a directory)
if [ -f $file ]
then
  echo "File is an ordinary file"
else
  echo "This is a special file"
fi

# Check if the file is a directory
if [ -d $file ]
then
  echo "File is a directory"
else
  echo "This is not a directory"
fi

# Check if the file size is greater than zero
if [ -s $file ]
then
  echo "File size is not zero"
else
  echo "File size is zero"
fi

# Check if the file exists
if [ -e $file ]
then
  echo "File exists"
else
  echo "File does not exist"
fi
```

# Explanation:

1. `[ -r $file ]`: Checks if the file has read permission. If true, the file has read access; otherwise, it doesn't.
   
2. `[ -w $file ]`: Checks if the file has write permission.

3. `[ -x $file ]`: Checks if the file has execute permission.

4. `[ -f $file ]`: Checks if the file is a regular (ordinary) file, meaning it's not a directory or special file.

5. `[ -d $file ]`: Checks if the file is a directory.

6. `[ -s $file ]`: Checks if the file's size is greater than zero, meaning the file contains data.

7. `[ -e $file ]`: Checks if the file exists in the specified location.

---


# Real-Time Use Case in DevOps:
---------------------------------

#### 1. Before modifying configuration files:
Before making changes to a configuration file during a deployment, you might want to ensure that the file exists and has the correct permissions.

```bash
#!/bin/bash
config_file="/etc/myapp/config.yml"

if [ -e $config_file ] && [ -w $config_file ]; then
  echo "Config file exists and has write access. Proceeding with changes."
  # Add commands to modify the config file
else
  echo "Config file is missing or not writable. Aborting."
  exit 1
fi
```

#### 2. Check if log files are growing:
You can use file size checks to monitor log files during automation, ensuring they are being updated.

```bash
#!/bin/bash
log_file="/var/log/myapp.log"

if [ -s $log_file ]; then
  echo "Log file has data."
else
  echo "Log file is empty."
fi
```

---

# Conclusion:

File test operators are essential when working with files in shell scripting, especially in DevOps workflows. They help automate tasks by validating the properties of files, such as checking for read/write permissions, existence, and file type before performing actions like file updates or backups.





# If.. Elif.. Else Statement
-----------------------------

The if..elif..else statement is used to check multiple conditions in a sequence. If the first condition is true, it runs the associated code block. If the first condition is false, it checks the next condition. If all conditions are false, the code in the else block is executed.

#### Syntax:
```bash
if [ condition ]; then
  # code to run if condition is true
elif [ condition ]; then
  # code to run if second condition is true
else
  # code to run if all conditions are false
fi
```

#### Example:
```bash
#!/bin/bash

var1=3
var2=3

if [ $var1 -gt $var2 ]; then
  echo "var1 is greater than var2"
elif [ $var1 -lt $var2 ]; then
  echo "var1 is lesser than var2"
else
  echo "var1 is equal to var2"
fi
```

### Real-Time Use Case:
Consider a deployment script where different actions are taken based on server load:

```bash
#!/bin/bash

load=$(uptime | awk '{print $10}' | sed 's/,//')  # Get server load value

if (( $(echo "$load > 5" |bc -l) )); then
  echo "Server under high load, delaying deployment."
elif (( $(echo "$load > 3" |bc -l) )); then
  echo "Server load is moderate, proceed with caution."
else
  echo "Server load is low, proceeding with deployment."
fi
```

This example checks the server load and makes decisions based on whether the load is high, moderate, or low, adjusting deployment behavior accordingly.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Switch (Case) Statement
---------------------------

The switch statement (also called case) allows you to compare a single variable or expression against multiple patterns. It is an alternative to using multiple if..elif..else conditions when checking for different possible values of a variable.

 Syntax:
```bash
case variable in
  Pattern1)
    # code to run if pattern1 matches ;;
  Pattern2)
    # code to run if pattern2 matches ;;
  )
    # code to run if none of the patterns match ;;
esac
```

#### Example:
```bash
#!/bin/bash

operation=$1  # reading the operation type

if [ $# -ne 1 ]; then
   echo "Please pass an argument: add | subtract | multiply"
   exit 1
fi

echo "Enter first value:"
read var1

if [ -z "$var1" ]; then
  echo "Please enter a valid first number"
  exit 1
fi

echo "Enter second value:"
read var2

if [ -z "$var2" ]; then
  echo "Please enter a valid second number"
  exit 1
fi

case "$operation" in
  "add")
    echo "Adding numbers"
    value=$(expr $var1 + $var2)
    echo "value=$value"
    ;;
  "subtract")
    echo "Subtracting numbers"
    value=$(expr $var1 - $var2)
    echo "value=$value"
    ;;
  "multiply")
    echo "Multiplying numbers"
    value=$(expr $var1 \ $var2)
    echo "value=$value"
    ;;
  )
    echo "Invalid operation"
    ;;
esac
```

# Real-Time Use Case:
----------------------
Consider a script that accepts different types of backup operations (full, incremental, differential) and performs the corresponding action:

```bash
#!/bin/bash

backup_type=$1  # full, incremental, differential

if [ -z "$backup_type" ]; then
  echo "Please specify a backup type: full | incremental | differential"
  exit 1
fi

case "$backup_type" in
  "full")
    echo "Performing full backup..."
    # Full backup command here
    ;;
  "incremental")
    echo "Performing incremental backup..."
    # Incremental backup command here
    ;;
  "differential")
    echo "Performing differential backup..."
    # Differential backup command here
    ;;
  )
    echo "Invalid backup type specified"
    ;;
esac
```

This example would allow you to run a backup script with different types of backups based on the argument passed.

---

# Run Examples:

- For If..Elif..Else: Save the script as `ifelif.sh` and run it:
  ```bash
  ./ifelif.sh
  ```

- For Switch (Case): Save the script as `all_example.sh` and run it with different operations:
  ```bash
  ./all_example.sh add
  ./all_example.sh subtract
  ```










# Boolean Operators:
Boolean operators are used to test multiple conditions in a script. The -a operator checks if both conditions are true (AND), and the -o operator checks if at least one condition is true (OR).

# Example:

```bash
#!/bin/bash

a=20
b=20

if [ $a -lt 100 -a $b -gt 15 ]; then
  echo "$a -lt 100 -a $b -gt 15 : returns true"
else
  echo "$a -lt 100 -a $b -gt 15 : returns false"
fi

if [ $a -lt 100 -a $b -gt 25 ]; then
  echo "$a -lt 100 -a $b -gt 25 : returns true"
else
  echo "$a -lt 100 -a $b -gt 25 : returns false"
fi

if [ $a -lt 100 -o $b -gt 100 ]; then
  echo "$a -lt 100 -o $b -gt 100 : returns true"
else
  echo "$a -lt 100 -o $b -gt 100 : returns false"
fi

if [ $a -lt 5 -o $b -gt 100 ]; then
  echo "$a -lt 100 -o $b -gt 100 : returns true"
else
  echo "$a -lt 100 -o $b -gt 100 : returns false"
fi
```

# Explanation:
- -a (AND): Both conditions must be true for the result to be true.
- -o (OR): At least one of the conditions must be true for the result to be true.



# Real-Time Scenario:
------------------------
Boolean operators can be useful in scripts where multiple conditions need to be validated, such as validating user input in a script:

```bash
#!/bin/bash

echo "Enter a number between 10 and 50:"
read num

if [ $num -ge 10 -a $num -le 50 ]; then
  echo "Valid input"
else
  echo "Invalid input. Please enter a number between 10 and 50."
fi
```

This script checks whether the input number falls within the specified range using the -a (AND) operator.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Shell Script Loops:
------------------------

Loops in shell scripting allow you to execute a block of code repeatedly based on certain conditions. This is useful when you need to perform repetitive tasks, such as processing multiple files or performing calculations over a range of values.

# Key Concepts:
- Loop: A control structure that repeatedly executes a block of code.
- Iteration: Each time the code block is executed, it's called an iteration.
- Condition: Determines whether the loop should continue running or terminate.


# Types of Loops:

---

1. While Loop:
A `while` loop runs as long as the condition remains true. It checks the condition before each iteration, and if the condition is false at the start, the loop won't execute.

#### Syntax:
```bash
while [ condition ]
do
  statement(s)
done
```

Example:
```bash
#!/bin/bash
# Printing numbers from 1 to 9

count=1
while [ $count -le 9 ]
do
  echo "count = $count"
  count=$((count + 1))  # Increment the counter
done

echo "-- End of loop --"
```

Explanation:
- The loop checks if the value of `count` is less than or equal to 9.
- If true, it prints the current value of `count` and then increments it.
- This continues until `count` becomes 10, at which point the condition is false, and the loop terminates.


Real-World Scenario:
Suppose you want to monitor a log file in real-time and perform an action when a certain string appears. A `while` loop can keep checking the file and stop when the desired string is found.

---


2. Until Loop:
The `until` loop is similar to the `while` loop, but it runs as long as the condition is false. Once the condition becomes true, the loop stops.

Syntax:

```bash
until [ condition ]
do
  statement(s)
done
```

#### Example:
```bash
#!/bin/bash

a=13  # Initial value
until [ $a -lt 10 ]
do
  echo "a=$a"
  a=$((a - 1))  # Decrement the value of a
done
```

Explanation:
- The loop will run until the value of `a` is less than 10.
- The `until` loop checks the condition after each iteration, and as soon as the condition becomes true, it exits the loop.


Real-World Scenario:
An `until` loop could be used when you're waiting for a service to start. You could keep checking the status of the service, and as soon as it's running, the loop exits.

---

3. For Loop:
The `for` loop allows you to iterate over a list of values. It is ideal when you have a predefined set of items you want to process.

Syntax:

```bash
for variable in val1 val2 val3
do
  statement(s)
done
```

Example:
```bash
#!/bin/bash

# Looping through a list of numbers
for i in 1 2 3 4 5
do
  echo "Looping ... number $i"
done
```

Explanation:
- The loop iterates over the values `1, 2, 3, 4, 5`.
- For each iteration, the value of `i` changes, and the code inside the loop is executed.

Real-World Scenario:
A `for` loop can be used to process multiple files in a directory. For example, if you want to compress all `.txt` files, you can use a `for` loop to iterate over each file and run the compression command.

```bash
#!/bin/bash
for file in .txt
do
  gzip "$file"  # Compress each .txt file
done
```

---

Conclusion:
Loops are powerful tools in shell scripting that allow you to perform repetitive tasks efficiently. The type of loop you choose depends on the scenario—whether you're working with a condition (`while` or `until` loops) or iterating over a set of values (`for` loop).





Here are some real-world DevOps scenarios where loops are commonly used in shell scripts:
--------------------------------------------------------------------------------------------

1. Monitoring System Resources:
In a DevOps environment, you might need to continuously monitor system resources (e.g., CPU, memory, disk usage) and take action if certain thresholds are crossed. A `while` loop can be used for this purpose.

Example:

```bash
#!/bin/bash
# Continuously monitor CPU usage
while true
do
  CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')
  if (( $(echo "$CPU_USAGE > 80.0" | bc -l) )); then
    echo "High CPU usage detected: $CPU_USAGE%"
    # Trigger alert or scale out resources
  fi
  sleep 60  # Check every 60 seconds
done
```
Use Case: Automating the scaling of resources when the server is under heavy load in a production environment.

---

2. Automated Backup of Logs or Data:
In continuous integration/continuous deployment (CI/CD), logs are crucial for debugging. A `for` loop can be used to back up or archive logs from multiple services.

Example:

```bash
#!/bin/bash
# Archiving logs for all services
for service in nginx apache tomcat
do
  tar -czf /backup/"$service"_log_$(date +%F).tar.gz /var/log/$service
  echo "Backup created for $service"
done
```
Use Case: Automating log backups in Jenkins jobs for multiple services after every deployment.

---

3. Deploying to Multiple Servers:
When deploying code or updates across multiple servers in a distributed environment, you can use a `for` loop to automate the process.

Example:

```bash
#!/bin/bash
# Deploy updates to multiple servers
for server in server1.example.com server2.example.com server3.example.com
do
  ssh $server 'sudo systemctl restart myapp'
  echo "Restarted myapp on $server"
done
```
Use Case: Deploying application updates across multiple environments in a production setup.

---

4. Continuous File Watcher for Configuration Changes:
In DevOps, keeping track of configuration changes is important. A `while` loop can monitor configuration files and trigger a restart if changes are detected.

Example:

```bash
#!/bin/bash
# Monitor Nginx config file for changes
while inotifywait -e modify /etc/nginx/nginx.conf
do
  echo "nginx.conf modified. Restarting Nginx."
  sudo systemctl restart nginx
done
```
Use Case: Automatically restart services when configuration changes are detected, ensuring seamless updates.

---

5. CI/CD Pipeline for Testing and Deployment:
During CI/CD pipelines, you can use loops to run test cases against different environments or branches.

Example:

```bash
#!/bin/bash
# Run tests on multiple environments
for env in dev staging prod
do
  echo "Running tests on $env"
  ./run_tests.sh --env=$env
done
```
Use Case: Running tests in different environments before promoting changes to production.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Loop Control in Shell Scripts:
---------------------------------

In shell scripting, loops help us run the same block of code multiple times, depending on a condition or a set of values. Sometimes, we need more granular control over when to stop or skip certain iterations. This is where loop control comes in.

Loop control is essential when dealing with scenarios where:

- You want to stop the loop based on a condition, even before it completes all its iterations (using `break`).
- You want to skip certain iterations without terminating the loop (using `continue`).

Let’s break down when to use different types of loops and loop control, followed by real-world DevOps scenarios.

---

# Choosing the Right Loop
Before jumping into loop control, consider the type of loop you want to use depending on your scenario:

1. While Loop:
   Use this when you have a condition and you want to execute code as long as that condition is true.

   Example:
   ```bash
   # Print numbers until the count is less than or equal to 10
   count=1
   while [ $count -le 10 ]
   do
     echo "count = $count"
     count=$((count + 1))
   done
   ```

2. Until Loop:
   Use this when you want to execute code until a condition becomes true. It runs while the condition is false.

   Example:
   ```bash
   # Countdown until a reaches below 10
   a=20
   until [ $a -lt 10 ]
   do
     echo "a = $a"
     a=$((a - 1))
   done
   ```

3. For Loop:
   Use this when you have a set of values and want to run the code a specific number of times based on the number of values in the list.

   Example:
   ```bash
   # Loop through a list of values
   for name in Alice Bob Carol
   do
     echo "Hello, $name!"
   done
   ```

---


# Loop Control: `break` and `continue`
----------------------------------------

1. Breaking Out of a Loop
   The `break` command allows you to exit a loop when a certain condition is met. This is useful when you want to stop the loop before it completes its natural course.

   Real-World Example:
   In DevOps, you may be monitoring a server’s status in a loop, and once a server becomes unreachable, you might want to exit the monitoring loop.

   Example:
   ```bash
   # Monitor server until a certain condition is met (e.g., high CPU usage)
   usage_threshold=80
   while true
   do
     CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')
     echo "Current CPU Usage: $CPU_USAGE%"
     if [ $(echo "$CPU_USAGE > $usage_threshold" | bc) -eq 1 ]; then
        echo "CPU usage exceeded threshold! Exiting..."
        break
     fi
     sleep 60
   done
   ```

   - Use Case: If the CPU usage goes above a certain limit, the loop terminates, allowing you to take corrective action or trigger alerts.


2. Skipping Iterations with `continue`
   The `continue` command skips the current iteration and moves to the next iteration without terminating the entire loop. This is useful when you want to bypass certain conditions but still complete the loop.

   Real-World Example:
   If you’re checking log files for errors in a CI/CD pipeline, but you want to skip any iteration where the log file is empty, you can use `continue` to skip to the next file.

   Example:
   ```bash
   # Skip iterations where no errors are found in log files
   LOG_FILES="/var/log/app1.log /var/log/app2.log /var/log/app3.log"

   for log in $LOG_FILES
   do
     if [ ! -s $log ]; then
        echo "$log is empty. Skipping..."
        continue
     fi
     echo "Checking $log for errors..."
     grep "ERROR" $log
   done
   ```

   - Use Case: If a log file is empty, it skips checking that file and moves on to the next one, saving time.

---

Practical Scenario: Automating Software Deployment with Loop Control

Scenario:
Let’s say you're automating the deployment of an application across multiple servers in a CI/CD pipeline. You want to:
1. Stop the deployment if any server is unreachable.
2. Skip any server that is already running the latest version of the application.


# Solution:

```bash
#!/bin/bash
# List of servers to deploy the app
SERVERS="server1.example.com server2.example.com server3.example.com"
APP_VERSION="2.0"

for server in $SERVERS
do
  echo "Deploying to $server..."

  # Check if server is reachable
  ping -c 1 $server > /dev/null 2>&1
  if [ $? -ne 0 ]; then
    echo "$server is unreachable. Stopping deployment!"
    break
  fi

  # Check if the server already has the latest version
  current_version=$(ssh $server 'cat /opt/myapp/version.txt')
  if [ "$current_version" == "$APP_VERSION" ]; then
    echo "$server already has version $APP_VERSION. Skipping deployment..."
    continue
  fi

  # Proceed with deployment
  echo "Deploying version $APP_VERSION to $server..."
  ssh $server 'sudo systemctl restart myapp'

done

echo "Deployment process completed."
```

- Break: If any server is unreachable, the script stops further deployment attempts.
- Continue: If the server already has the desired version, it skips deploying to that server and moves to the next one.

---

Conclusion

In DevOps automation, loop control is vital for handling real-time dynamic conditions. Whether you are:
- Monitoring systems, 
- Automating deployments, or 
- Managing logs,

loops with proper control (`break` and `continue`) provide flexibility to handle complex scenarios, helping ensure efficiency and accuracy in your automation scripts.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Nested Loops in Shell Scripting:
-----------------------------------

Nested loops refer to loops within loops, where you are running a loop inside another loop. This can be useful when you want to repeat a set of operations multiple times within the context of an outer loop.

Example 1: Nested Loops

```bash
#!/bin/bash
set -vx

a=0
b=0

# Outer while loop
while [ $a -lt 5 ]
do
  b=1
  char=""

  # Inner while loop
  while [ $b -le $a ]
  do
     char=" $char"
     b=`expr $b + 1`
  done

  echo "$char"
  a=`expr $a + 1`
done

echo "--------"

# Another outer loop using until
until [ $a -lt 0 ]
do
  c=1
  char=""

  # Nested while loop inside until
  while [ $c -lt $a ]
  do
     char=" $char"
     c=`expr $c + 1`
  done

  echo "$char"
  a=`expr $a - 1`
done
```


# Explanation:
- The first while loop iterates 5 times, and for each iteration, the inner loop runs to create a line of `` characters, increasing by one `` in each iteration.
- After completing the first set of loops, the until loop starts and runs while the condition `a -lt 0` becomes true, again using a nested while loop to print a line of stars, reducing the number of stars on each line.


Example 2: Averages Using Loops

Here’s another example that combines user input with loops, allowing users to enter numbers until they decide to stop. Then it calculates the average of the entered numbers.

```bash
#!/bin/bash
#- Print average of given numbers -#

sum=0
ans="y"
count=0

while [ $ans == "y" ]
do
  # Only prompt to continue after the first iteration
  if [ $count -ne 0 ]; then
     read -p "Do you want to continue (y/n):" ans
  fi

  # Collect numbers if the user wants to continue
  if [ $ans == "y" ]; then
     read -p "Enter a number:" num
     sum=`expr $sum + $num`
     count=`expr $count + 1`
  fi
done

# Check if there are enough values for an average
if [ $count -lt 2 ]; then
   echo "You need at least 2 values for the average"
else
   avg=`expr $sum / $count`
   echo "Average = $avg"
fi
```

Explanation:
- This script uses a while loop that allows users to enter numbers until they decide to stop.
- It calculates the sum of the numbers and keeps a count of how many numbers have been entered.
- The loop prompts the user after each iteration to decide whether they want to continue entering numbers.
- Once the user exits, it calculates the average if there are at least two numbers.

---

These examples show how loops, including nested loops, can be used for repetitive tasks in shell scripts, allowing more control over execution flow. This is especially helpful in automating tasks in DevOps, where monitoring or repeated actions across servers or data sets are common.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Shell Functions:
-------------------

Shell functions are powerful tools that allow you to organize your scripts into smaller, reusable parts. They help improve code readability and maintainability by breaking down larger scripts into manageable sections. 

# Benefits of Using Functions:
- Code Reuse: Once a function is defined, you can call it multiple times throughout your script without rewriting the same code.
- Modularity: Functions help divide complex scripts into smaller, more understandable pieces.


# Defining a Function

The syntax for defining a function in a shell script is as follows:

```bash
function_name () {
   # statement(s)
}
```

# Calling a Function

To call a function, use its name followed by any arguments you want to pass:

```bash
function_name <arguments>
```

# Purpose of Functions

In a script, you might have a set of code that you want to execute multiple times. While loops can repeat code, functions are more about reusability. When working on larger automation scripts, using functions to break down the code into smaller activities makes it easier to manage and debug.


# Example of a Basic Function

```bash
#!/bin/bash

echo "Starting the script"

# Define the function
hello () {
  echo "I'm inside the Hello() function"
  echo "Hello, ADAM!"
}

echo "Calling the function..."
hello
echo " -- End of script --"
hello
```

# Explanation:
- The script defines a function named `hello`, which prints a message.
- After defining the function, it is called twice within the script, demonstrating the reusability of the code.




# Advanced Concepts of Functions:
-----------------------------------

Functions can also accept parameters and return values. 

# Passing Parameters
You can pass parameters to a function using the following syntax:

```bash
function_name <param1> <param2>
```

Inside the function, these parameters can be accessed using `$1`, `$2`, etc.

#### Returning Values
To return a value from a function, use the `return` statement:

```bash
return <value>
```


# Example with Parameters
--------------------------

```bash
#!/bin/bash

a=1
b=1

check () {
  echo "Going to check the values: $1 and $2"

  if [ $1 -eq $2 ]; then
    echo "Numbers are equal"
  else
    echo "Numbers are not equal"
  fi
}

echo "Calling check() function with values $a and $b..."
check $a $b

c=2
d=3
echo "Calling check() function again with values $c and $d..."
check $c $d

echo " -- End of script --"
```

 Explanation:
- In this script, a function named `check` is defined to compare two numbers.
- It takes two parameters and checks if they are equal or not.
- The script demonstrates calling the `check` function with different sets of values.


# Real-World Application in DevOps:

In DevOps, functions can be invaluable for automating repetitive tasks. For instance, if you're deploying applications across multiple environments, you could define a function to handle deployment steps (e.g., pulling the latest code, building the application, and restarting services). This function can be reused in different deployment scripts, ensuring consistency and saving time. 

Using functions to handle configuration checks, backups, or monitoring tasks can streamline operations and reduce the chance of errors in scripts. By dividing scripts into functions, teams can collaborate more effectively, as different team members can work on separate functions without interfering with the overall script structure.





# Shell Functions with Return Values:
---------------------------------------

In shell scripting, functions not only help organize your code but can also perform calculations and return values. The logic you implement within the function is flexible, allowing you to define various tasks based on your needs. 

# Defining a Function with Return Value

You can create a function that performs a specific task, such as adding two numbers, and return the result. To achieve this, you must include the `return` keyword at the end of your function.


# Example: Calculating Average

Here’s an example of a shell script that calculates the average of a series of numbers using a function:

```bash
#!/bin/bash

echo "Starting the script"

avg () {
  sum=0

  # Loop through all arguments passed to the function
  for var in "$@"; do
    sum=$(expr $sum + $var)  # Sum the numbers
  done
  
  echo "Sum of $@ = $sum"
  
  # Calculate average
  val=$(expr $sum / $#)
  
  # Return the average value
  return $val
}

# Call the avg function with different sets of numbers
avg 1 2
ret=$?
echo -e "Average of 1 2 == $ret \n"

avg 3 4 5
ret=$?
echo -e "Average of 3 4 5 == $ret \n"
```

Explanation:
1. Function Definition:
   - The `avg` function initializes a variable `sum` to 0.
   - It uses a `for` loop to iterate over all the arguments passed to the function (denoted by `$@`), adding each number to `sum`.

2. Calculating the Average:
   - After calculating the total sum, the function computes the average by dividing the sum by the number of arguments (`$#`).
   - The `return` statement is used to return the average value, which can be accessed using the special variable `$?` after the function call.

3. Function Calls:
   - The `avg` function is called twice with different sets of numbers. The results are captured using the `ret` variable.

4. Output:
   - The script prints the sum of the numbers and the calculated average.


# Real-World Application in DevOps:
------------------------------------

In a DevOps context, such functions can be very useful for automating reporting tasks. For instance, you might have a function that calculates the average response time of your services based on metrics collected during performance tests. This average could then be used in monitoring dashboards or alerts.

By encapsulating such logic in functions, you can easily reuse and maintain them across different scripts, helping you streamline your DevOps processes and ensure consistency in how metrics are calculated and reported.






# Calling Functions Within Functions:
----------------------------------------

In shell scripting, you can call one function from another, creating a parent-child relationship between them. This technique allows for more modular and organized code. You can also pass arguments from one function to another, enhancing their utility.

### Example: Nested Function Calls

Here’s an example that demonstrates calling a child function from a parent function:

```bash
#!/bin/sh

# Define the first function
number_one () {
  echo "This is the first function speaking ..."
  
  # Call the second function
  number_two
}

# Define the second function
number_two () {
  echo "This is now the second function speaking ..."
}

# Calling the first function
number_one
```

Explanation:
1. Function Definitions:
   - The `number_one` function outputs a message and then calls the `number_two` function.
   - The `number_two` function simply outputs its own message.

2. Function Call:
   - When you call `number_one`, it executes its code, which includes a call to `number_two`. As a result, both messages are printed sequentially.



# Passing Arguments

You can also modify this example to pass arguments from the parent function to the child function. Here’s how you can achieve that:

```bash
#!/bin/sh

# Define the first function
number_one () {
  echo "This is the first function speaking ..."
  
  # Call the second function and pass an argument
  number_two "Hello from number_one!"
}

# Define the second function
number_two () {
  echo "This is now the second function speaking ..."
  echo "Received message: $1"  # Print the received argument
}

# Calling the first function
number_one
```

# Explanation of Argument Passing:
1. Modified Function Call:
   - In `number_one`, when calling `number_two`, we pass a string argument `"Hello from number_one!"`.

2. Receiving Arguments:
   - Inside `number_two`, we access the passed argument using `$1`, which represents the first argument.



# Real-World Application in DevOps

In a DevOps environment, using nested functions can help manage complex scripts by breaking them down into manageable parts. For example, you might have a function responsible for gathering system metrics and another function that processes and logs those metrics. By calling one function from another, you can create a structured workflow that improves readability and maintainability of your scripts.

This approach allows you to easily modify and extend your scripts, as changes to one function won't necessarily impact others, leading to more robust automation in your DevOps practices.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Shell Script Redirections:
-----------------------------

Redirections in shell scripting allow you to control where the output of your commands goes. Instead of displaying output on the screen, you can write it to files, discard it, or run scripts in the background. This can be particularly useful for logging, automation, and managing long-running scripts.

# Types of Redirection

1. Output Redirection:
   - Overwrite Output: Use `>` to redirect the output to a file, overwriting the existing content.
     ```bash
     ./script.sh > output.txt
     ```
   - Append Output: Use `>>` to append the output to a file without overwriting existing content.
     ```bash
     ./script.sh >> output.txt
     ```

2. Discard Output:
   - To completely discard the output of a command, redirect it to `/dev/null`. This is useful for suppressing output from commands you don’t want to see.
     ```bash
     ./script.sh > /dev/null
     ```

3. No Hang Up (nohup):
   - The `nohup` command allows a script to run in the background, immune to hangups. This is useful for long-running processes, ensuring they continue even if the terminal is closed.
   - You can combine `nohup` with redirection to capture both standard output and error output in a file.
     ```bash
     nohup script.sh > output.log 2>&1 &
     ```


# Example of Output Redirection

Here’s a simple script that prompts for a name and stores the output in a log file:

```bash
#!/bin/bash

read -p "Enter your name: " name
echo "Hello $name !!" > log.txt
echo "Hello $name - Welcome to Shell Scripting!" >> log.txt

echo " -- End of Script -- "
```

When you run this script, it will ask for your name and save the output in `log.txt`. If you run it multiple times, using `>>` ensures that previous entries are not overwritten.



# Discarding Output Example

Consider the following script that produces both output and an error:

```bash
#!/bin/bash

echo "Line 1"
ls nonexistentfile  # This will produce an error
echo "Line 2"
```

- Running `./script.sh` will display both lines on the screen, along with the error message.
- Running `./script.sh > /dev/null` will suppress all output, including errors.



# Running Scripts in the Background

To run a script that might take a long time or might be interrupted, use `nohup`:

```bash
#!/bin/bash

num=1
while true; do
  echo "Number is $num"
  num=$(expr $num + 1)
done
```

Run it using:
```bash
nohup ./long_running_script.sh > output.log 2>&1 &
```

This way, even if you log out, the script continues running, and any output is captured in `output.log`.



# Real-World Scenario

In a DevOps environment, you might automate server health checks using a shell script. For example, a script that checks disk space, memory usage, and CPU load can run regularly in the background. 

Using redirection:
- You could redirect the output to a log file for historical analysis.
- If a particular check fails (like disk space running low), you might want to suppress all other output but log the error to a different file for alerting purposes.
- By using `nohup`, you can ensure that the health check script continues to run even if the session is disconnected, allowing for continuous monitoring of your system.

This approach improves reliability and allows for better management of system resources, ensuring that you can react to potential issues without needing constant manual oversight.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Capturing Output in Shell Scripts:
--------------------------------------

Capturing output in shell scripting refers to the process of executing a command and storing its output in a variable for further use within the script. This technique allows you to leverage command results dynamically, enabling more complex operations and conditional logic.

# Why Capture Output?

1. Dynamic Value Handling: You may need to perform operations based on the current state of the system, such as counting files, checking disk space, or capturing process IDs.
2. Reuse of Command Results: Capturing output allows you to reuse results without executing the command multiple times, which can improve performance.
3. Conditional Logic: You can make decisions in your script based on the output captured, enhancing the script's functionality.


# Methods for Capturing Output

1. Using Backticks (` `):
   Backticks allow you to execute a command and capture its output. However, this method has some limitations, especially with nested commands or when using complex syntax.

   Example:
   ```bash
   # output.sh
   #!/bin/bash

   var=`ls | wc -l`  # Backticks to capture output
   echo "Total number of files in current directory == $var"
   ```

   - In this example, `ls` lists the files in the current directory, and `wc -l` counts them. The total count is stored in the variable `var`.

   Limitations:
   - Backticks do not handle nested commands well, making them less flexible for more complex scripts.

2. Using Command Substitution with Parentheses $( ):
   This is the preferred method for capturing output in modern shell scripting. It is more readable, supports nested commands, and avoids issues with spaces and quotes.

   Example:
   ```bash
   # output.sh
   #!/bin/bash

   var=$(ls | wc -l)  # Using $( ) for capturing output
   echo "Total number of files in current directory == $var"
   ```

   This accomplishes the same task as the previous example but uses command substitution, which is generally recommended for better readability and functionality.

3. Nested Command Substitution:
   When you need to capture output from nested commands, use the `$( )` syntax. This allows you to execute multiple commands in a single expression.

   Example:
   ```bash
   # output.sh
   #!/bin/bash

   var=$(ls | wc -l)  # Count files in the current directory
   echo "Total number of files in current directory == $var"

   var1=$(echo Today is $(date))  # Capture date output
   echo "var1 = $var1"
   ```

   - In this script, `$(date)` captures the current date and time. The result is then combined with a string to produce a meaningful output.



# Advantages of Using `$( )` Over Backticks

- Readability: Using parentheses makes it clear where the command substitution starts and ends.
- Nested Commands: `$( )` easily supports nested command execution without confusion.
- Error Handling: It handles errors more gracefully, allowing better script maintenance.


# Example Use Cases

1. File Count:
   You might need to count files in a directory and take action based on that count, such as backing up files if the count exceeds a threshold.

   ```bash
   # backup.sh
   #!/bin/bash

   file_count=$(ls | wc -l)
   if [ "$file_count" -gt 10 ]; then
       echo "Backing up files..."
       # Command to back up files
   else
       echo "No backup needed."
   fi
   ```

2. Dynamic Reporting:
   Capture various system states (like memory usage or disk space) and generate a report.

   ```bash
   # report.sh
   #!/bin/bash

   disk_usage=$(df -h | grep '^/dev/' | awk '{print $5}')  # Capture disk usage percentage
   echo "Current disk usage: $disk_usage"
   ```

3. Logging Activities:
   Capture timestamps for logging activities in a script.

   ```bash
   # log_activity.sh
   #!/bin/bash

   log_file="activity.log"
   current_time=$(date +"%Y-%m-%d %H:%M:%S")
   echo "Activity logged at $current_time" >> $log_file
   ```

# Conclusion

Capturing output in shell scripts is a fundamental skill for effective scripting. It enables dynamic behavior, allows for efficient resource management, and can significantly enhance the capabilities of your scripts. By utilizing command substitution with `$( )`, you can build robust and maintainable shell scripts that perform complex tasks based on real-time data.




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


 Medium to Advanced Shell Scripting Interview Questions based on the concepts we've covered and a few additional ones
-----------------------------------------------------------------------------------------------------------------------


1. What is Shell Scripting, and why is it used?

Answer:  
Shell scripting is writing a sequence of commands for the Unix/Linux shell to execute. It is used to automate repetitive tasks, manage system operations, configure environments, and simplify complex tasks by running multiple commands in a single file. Shell scripts also help in job scheduling, backup, and administrative tasks.



2. What are different types of shells, and which one do you prefer? Why?

Answer:  
Common types of shells are:
- Bourne Shell (sh)
- Bash (Bourne Again Shell)
- C Shell (csh)
- Korn Shell (ksh)
- Z Shell (zsh)

Bash is most commonly used because it's widely supported, user-friendly, and provides powerful features like command history, command-line editing, and scripting capabilities.



3. Explain how redirection works in shell scripting.

Answer:  
Redirection allows the output of a command to be written to a file instead of the terminal. The basic syntax:
- `>`: Redirects output to a file, overwriting the file if it exists.
- `>>`: Appends output to the file without overwriting.
- `2>`: Redirects errors to a file.
- `/dev/null`: Discards output entirely.
- `2>&1`: Combines stdout and stderr into one output stream.

Example:
```bash
ls > output.txt        # Redirect output to a file
ls >> output.txt       # Append output to the same file
ls 2> error.log        # Redirect errors to a log file
ls > /dev/null         # Discard the output
```



4. What are positional parameters?

Answer:  
Positional parameters are special variables in a shell script that represent arguments passed to the script. `$1`, `$2`, ..., represent the first, second, etc., arguments, and `$#` gives the number of arguments passed.

Example:
```bash
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
```



5. How do you capture command output in a variable? Provide an example.

Answer:  
Command output can be captured using backticks (`` `command` ``) or `$()`.

Example:
```bash
output=$(ls | wc -l)
echo "Number of files: $output"
```

This stores the result of the `ls | wc -l` command in the variable `output`.



6. Explain how the `nohup` command works. When is it used?

Answer:  
The `nohup` command runs a script or command in the background, immune to hangups or terminal closures. It is typically used to keep processes running even if the session is closed.

Example:
```bash
nohup ./script.sh &
```
This command runs `script.sh` in the background and saves output to `nohup.out`.



7. How do you write and call a function within a shell script?

Answer:
```bash
#!/bin/bash

my_function() {
  echo "This is a function"
}

my_function  # Call the function
```



8. Can a function return a value? How is it captured?

Answer:  
Functions in shell scripts return an exit status (0-255), not a regular value. However, you can echo a value and capture it.

Example:
```bash
my_function() {
  echo $(( $1 + $2 ))
}

result=$(my_function 2 3)
echo "Sum is: $result"
```



9. How do you handle errors in a shell script?

Answer:  
Errors can be handled by checking the exit status (`$?`) of commands. You can also use `set -e` to exit the script if any command fails.

Example:
```bash
cp file1 file2
if [ $? -eq 0 ]; then
  echo "Copy successful"
else
  echo "Error occurred during copy"
fi
```



10. What is the use of `/dev/null` in shell scripting?

Answer:  
`/dev/null` is a special file that discards all data written to it. It is useful for discarding unnecessary output.

Example:
```bash
ls > /dev/null 2>&1
```
This discards both stdout and stderr.



11. What is the difference between `&` and `&&`?

Answer:  
- `&`: Executes a command in the background.
- `&&`: Executes the second command only if the first one is successful.

Example:
```bash
./script.sh &         # Run script in background
./script.sh && echo "Success"  # Echo "Success" only if script.sh succeeds
```



12. How do you schedule a script to run periodically?

Answer:  
You can schedule a script using cron jobs. Use `crontab -e` to add jobs.

Example (run every day at midnight):
```bash
0 0    /path/to/script.sh
```



13. Explain the difference between `$@` and `$`.

Answer:  
- `$@`: Expands arguments as separate quoted strings.
- `$`: Expands all arguments as a single string.

Example:
```bash
#!/bin/bash
for arg in "$@"; do
  echo "$arg"
done

for arg in "$"; do
  echo "$arg"
done
```



14. What are exit statuses, and how are they used in a script?

Answer:  
Exit statuses are numeric codes returned by commands. `0` indicates success, while non-zero indicates failure.

Example:
```bash
#!/bin/bash
cp file1 file2
if [ $? -eq 0 ]; then
  echo "Copy successful"
else
  echo "Copy failed"
fi
```



15. How would you debug a shell script?

Answer:  
You can debug a shell script using `set -x` for tracing and `set -e` to exit on errors.

Example:
```bash
#!/bin/bash
set -x  # Start tracing
cp file1 file2
set +x  # Stop tracing
```



16. What is a shebang? Why is it important?

Answer:  
The shebang (`#!/bin/bash`) is the first line of a script that specifies the path to the interpreter. It ensures the correct shell is used to run the script.



17. What is the significance of `2>&1`?

Answer:  
`2>&1` redirects stderr (file descriptor 2) to stdout (file descriptor 1). This ensures both standard output and errors are combined.

Example:
```bash
./script.sh > output.log 2>&1
```



18. How do you perform arithmetic operations in shell scripts?

Answer:
```bash
#!/bin/bash
result=$(( 2 + 3 ))
echo "Sum is: $result"
```

You can also use `expr` for arithmetic operations:
```bash
result=`expr 2 + 3`
```



19. What is a Here Document? How do you use it?

Answer:  
A Here Document (`<<`) is used to provide multi-line input to a command or script.

Example:
```bash
cat << EOF
This is a here document.
EOF
```



20. What is the difference between `test` and `[ ]`?

Answer:  
`test` and `[ ]` are equivalent. Both are used for evaluating expressions in shell scripts.

Example:
```bash
if [ -f file.txt ]; then
  echo "File exists"
fi
```



21. What is `shift` in shell scripting?

Answer:  
`shift` is used to shift positional parameters to the left. Each call to `shift` moves `$2` to `$1`, `$3` to `$2`, and so on.

Example:
```bash
#!/bin/bash
while [ "$1" != "" ]; do
  echo "Argument: $1"
  shift
done
```



22. Explain piping in shell scripting.

Answer:  
Piping (`|`) is used to send the output of one command as the input to another.

Example:
```bash
ls | grep ".txt"
```



23. What are some common file test operators in shell scripting?

Answer:  
- `-f`: Checks if a file exists and is a regular file.
- `-d`: Checks if a directory exists.
- `-r`: Checks if a file is readable.
- `-w`: Checks if a file is writable.
- `-x`: Checks if a file is executable.



24. What is the purpose of the `trap` command?

Answer:  
`trap` is used to capture signals and execute commands when the signal is received.

Example:
```bash
#!/bin/bash


trap "echo 'Signal received! Exiting...'; exit" SIGINT
while :; do
  echo "Running..."
  sleep 1
done
```



25. How do you check if a variable is set?

Answer:
```bash
if [ -z "${var}" ]; then
  echo "Variable is not set"
else
  echo "Variable is set"
fi
```



26. What are the differences between hard links and soft links?

Answer:
- Hard Links: Direct pointers to the inode of a file. Multiple hard links point to the same inode.
- Soft Links (Symlinks): Pointers to the filename, not the inode.



27. Explain the `find` command with an example.

Answer:  
`find` is used to search for files and directories based on conditions.

Example:
```bash
find /path/to/search -name ".txt"
```



28. How do you handle command-line options in a script?

Answer:  
Using `getopts` for handling options.

Example:
```bash
#!/bin/bash
while getopts "a:b:" opt; do
  case $opt in
    a) echo "Option a with argument: $OPTARG" ;;
    b) echo "Option b with argument: $OPTARG" ;;
  esac
done
```



29. How do you loop over a file line by line?

Answer:
```bash
#!/bin/bash
while IFS= read -r line; do
  echo "$line"
done < file.txt
```



30. How do you execute multiple commands in parallel?

Answer:  
You can use `&` to run commands in the background.

Example:
```bash
cmd1 & cmd2 & wait
```



