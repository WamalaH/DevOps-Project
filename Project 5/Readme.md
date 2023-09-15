# SHELL SCRIPTING 

Shell scripting helps a *DevOps* engineer to automate repetitive tasks. By simply writing a script, a *DevOps* engineer can call the script to do a job whenever he has to do the same task.

*Bash*    scripts are essentially a series of commands and instructions that are executed sequentially in a shell. We create a shell script by saving a collection in a text file with a *.sh* extension. These scripts can be executed directly from the command line or called from other scripts.


## Shell Scripting Syntax Elements ##

**Variables**

Variables can store data of various types such as numbers,strings and arrays. We can assign values to variables using the = operator and access their values using the variable name preceded by a $ sign.

![Alt text](<Images/echo name.png>)


**Control flow**

Bash provides control flow statements like if-else, for loops, while loops and case statements to control the flow of execution in our scripts. These statements allow us to make decisions , iterate over lists and excute different commands based on conditions.

![Alt text](<Images/iterating through a list.png>)

**Input and output**

Bash provides various ways to handle input and output. We can use the read command to accept user input and output text to the console using the echo command. Additionally we can redirect input and output using operators like >,(output to a file) and <(input to a file) | pipe the output of one command as input to another.

![Alt text](<Images/what is your name.png>)

### Samples of scripts and their outputs ###

> File Operations and sorting

This script creates three files ,displays the files in their current order,sorts them alphabetically ,saves the sorted files and removes the original files. It also renames the sorted files and finally dispalys the conents of the final sorted files.

![Alt text](<Images/file operations.png>)


> Calculations

This script defines two variables with numeric values and performs  basic arithemetic operations.

![Alt text](<Images/working with numbers.png>)