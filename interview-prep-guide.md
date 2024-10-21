
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



