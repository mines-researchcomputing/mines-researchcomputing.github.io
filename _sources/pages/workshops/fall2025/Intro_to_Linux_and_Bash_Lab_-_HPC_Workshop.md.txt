# Intro to Linux / Bash on HPC Lab

## Connecting to an HPC System via SSH

### Linux and MacOS
Connecting to and using Mines' HPC systems requires knowledge of the command line. The first step will be opening the terminal on your operating system. On Mac systems, the quickest way is to use the "Spotlight" search by pressing "Command + Space" on your keyboard and typing "Terminal". Most Linux systems will also have a terminal app preinstalled. Once you have the terminal open, you can login using the ssh command. If you are on the Mines network, either on-campus, or connected to the Mines VPN, you can type the command.d

	$ ssh username@name-of-hpc-system.mines.edu

and enter your Mines Multipass password.

For example, if you have an account on Wendian and your username is janedoe,

	$ ssh janedoe@wendian.mines.edu 

### Windows
On Windows, we either recommend using the Windows Terminal app. It is preinstalled on most Windows 11 systems, and available on Windows 10 via the [Microsoft Store](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701). Once you have this application, you can ssh using the Linux and MacOS instructions above.


## Introduction to Command Line and Filesystem
In this section, we will go through the basics of how to use the command line to navigate the filesystem. We will also briefly describe aspects of the filesystem that are important to keep your work organized and adhere to HPC policy.

### Filesystem Basics
For both HPC systems, there is a basic filesystem structure with designated paths for various types of files. Upon login to either Wendian or Mio, typing ls, you should see the following too directories: bins and scratch.

	Useful Environment Variables
	Directory 	Environmental variable
	Home directory 	$HOME
	$HOME/bins 	$BINS
	$HOME/scratch 	$SCRATCH

Your home directory is where you can store any personal files, scripts, and programs that you need to keep long term. The bins is a place to store compiled programs and executables that you need to run for jobs. As the name implies, scratch is the scratch directory used for temporary files needed for running jobs.

> It is HPC policy to make sure that job data is run under your scratch directory. Users who do not follow this may receive notice or warnings from HPC staff.



### Choosing a text editor
When using the command line, a text editor becomes a powerful tool to create and edit job scripts, as well as modify codes on the system. There are 3 text editors that we recommend:

* `nano` - recommended for beginners new to command line-based text editors. A guide and shortcuts can be found on their website.
* `vim` - A keyboard-centric highly customizable text editor. Recommended for advanced users who want to customize how they edit their files on the commandline.
* `emacs` - Another power user-friendly text editor. 

## **Lab Exercise #1: Navigating the Linux Command Line**
In this exercise, you will learn the basics of navigating the Linux command line, including how to move between directories, list directory contents, and understand the file system hierarchy.

**Exercise 1: Navigating the file system**

**Step 1: Open a Terminal**

1.1.	Launch your terminal emulator of choice based on your operating system:
-	Linux/macOS: Default terminal application
-	Windows 11: Windows Terminal should be preinstalled with ssh 

**Step 2: Navigating the File System**

2.1. The first thing we'll do is to understand where you are in the file system. In the terminal, type the following command and press Enter:
   
```bash
pwd
```

This will print the present working directory, showing your current location in the file system.

2.2. Next, let's list the contents of the current directory. Use the `ls` command:

```bash
ls
```

You will see a list of files and directories in your current location.

**Step 3: Changing Directories**

3.1. To navigate to a different directory, you can use the `cd` command followed by the directory path. For example, to change to the `~/scratch` directory (which contains system configuration files), type:

```bash
cd ~/scratch
```

3.2. Now that you are in the `~/scratch` directory, you can use `pwd` again to confirm your current location.

3.3. Let's list the contents of the `~/scratch` directory:

```bash
ls
```

You will see various configuration files and subdirectories.

**Step 4: Moving Up the Directory Tree**

4.1. To move back to the parent directory (in this case, your home directory), you can use a special symbol, `..`, which represents the parent directory. Type:

```bash
cd ..
```

4.2. Use `pwd` to confirm that you are back in your home directory.

**Step 5: Navigating to Your Home Directory**

5.1. You can also navigate directly to your home directory using the `cd` command without any arguments. Try it:

```bash
cd
```

You can also use `~` as a reference back to your home directory as well:

```bash
cd ~
```


5.2. Confirm your location with `pwd`.

**Step 6: Creating and Navigating Subdirectories**

6.1. Let's create a new directory within your home directory. You can use the `mkdir` command. For example, create a directory called `my_documents`:

```bash
mkdir my_documents
```

6.2. Navigate into the newly created directory:

```bash
cd my_documents
```

6.3. Use `pwd` to confirm your location.


**Step 7: Create a file in my_documents**

8.1 Use your text editor to open a new file. Below, we will use `nano`:

```bash
nano new_file.txt
```

Add the following text to the file, then save and close it.

```
QWERTY1234
```

With `nano`, on your keyboard press 'CTRL+X' then type 'y' and press enter to confirm.


**Step 8: Returning to Your Home Directory**

8.1. To go back to your home directory, you can use the `cd` command without any arguments again:

```bash
cd
```

**Step 9: Removing the Created Directory**

9.1. To remove the created directory, you first need to empty it:

```bash
rm new_file.txt
```


If you want to remove the `my_documents` directory, you can use the `rmdir` command:

```bash
rmdir my_documents
```


The table below summarizes all the basic commands you need to navigate the file system:

Command | Description |
---|---|
`ls -lah` |	shows list (`-l`) of all files/directories, including hidden ones (`-a`), in current directory with human-readable (`-h`) file sizes |
`pwd` |	prints out the path of the current working directory |
`cd`    |	changes your directory to your home directory (on most Linux/Mac machines, this will be `/home/username`) |
`cd ..` |	changes your directory down one directory on the path, e.g. if you're at `/home/username, "`cd ..`" will take you to `/home` |
`cd $directory-path` |	changes to directory at `$directory-path` |
`mkdir $directory-name` |	creates a directory in your current working directory called `$directory-name` |
`mkdir -p $directory-path` |	creates a directory located at `$directory-path` (e.g. `$directory-path="/home/Documents/"` would move you to `/home/Documents` as your current working directory) |
`cp $filename $new_file_location` |	copies `$filename` (e.g. `$filename="/home/Documents/foo.txt"` would copy the file to `$new_file_location` (e.g. `$new_file_location="/home/Documents/New/foo.txt")` |
`mv $filename $new_file_location` |	moves `$filename` (e.g. `$filename="/home/Documents/foo.txt"` would copy the file to `$new_file_location` (e.g. `$new_file_location="/home/Documents/New/foo.txt")`

    
**Exercise 2: Transfer a file to HPC**

On your local machine, create a text file and type of bunch of random text in it.


On Mac/Linux/WSL:

```bash
cd $HOME
nano random.txt
```


Now transfer the file to your home directory on Wendian:
```bash
scp random.txt <username>@wendian.mines.edu:~
```

## Lab #2: Introduction to Bash Scripting Lab


### Lab Exercises:

**Exercise 1: Basic Commands**

1. Open a terminal or command prompt.
2. Create a directory named `bash_lab` in your home directory using the command `mkdir`. 
3. Navigate to the `bash_lab` directory using the `cd` command.
4. Create a text file named `hello_world.txt` in the `bash_lab` directory using the text editor of your choice (`nano`, `vim`, `emacs` are recommended):
```bash
nano hello_world.txt
```

Write `Hello World!` in it:
```bash
Hello World!
````
Save and close the file. If you're using `nano`, use Ctrl + X and then type 'y' and press enter to save. 

5. List the contents of the directory to confirm that `hello_world.txt` was created using `ls`. Try `ls -h` and use flags to see how the output changes.
6. Display the contents of `hello_world.txt` on the terminal using the `cat` command. 
```bash
cat hello_world.txt
```

**Exercise 2: Variables and Echo**

1. Create a Bash script named `greet.sh` that stores your name in a variable and then echoes a greeting using that variable. 

Open and create the file using the text editor of your choice (common are `nano`, `vim`, `emacs`):
```bash
nano greet.sh
```

Add the following to the file:

```bash
#!/bin/bash

NAME="Your Name"

echo "Hello ${NAME}!"
```


2. Make the script executable using the `chmod` command:
```bash
chmod +x greet.sh
```

3. Execute the script to see the greeting. There's two ways to do this:

    * Use the notation `./`:

    ```bash 
    ./greet.sh
    ```

    * Use the `bash` command:
    
    ```bash
    bash greet.sh
    ```

**Exercise 3: Conditional Statements**

Write a Bash script that takes a number as input and tells whether it's even or odd.

Create a script called `even_odd.sh`, and then open it with your text editor. 

Populate it with the following text

```bash
#!/bin/bash   

# assign the number inputted by user in the command with my_number
my_number=$1

if [[ $((my_number%2)) -eq 0 ]]
then
    echo "The inputted number is even!"
else
    echo "The inputted number is odd!"
fi
```

Make it executable and then run it:
```
chmod +x even_odd.sh
./even_odd.sh <input a integer here>
```

Let's see if it works:
```bash
$ ./even_odd.sh 47
The inputted number is odd!
$ ./even_odd.sh 1002
The inputted number is even!
```

BONUS: Figure out how to print an error if a non-integer number is inputted, or if a non-numeric character is inputted. 

**Exercise 4: Loops**

Next, we're going to write a bash script that prints numbers from 1 to 10 using a `for` loop.

Create a file called `my_for_loop.sh` and add the following:

```bash
#!/bin/bash   

N=10
for (( i=1; i<=$N; i++ ))
do
    echo "$i"
done
```

We can also modify the script to use a `while` loop instead:

```bash
#!/bin/bash   

N=10
i=1
while [ $i -le $N ]
do 
    echo "$i"
    i=$(( $i +1 )) # Double parentheses are a C-style construct to apply arithmetic evaluation
done
```


## DEMO: Shell and Environment Variables

In Unix/Linux environments, you can set variables that can be used later.

For example, let's say you wanted set a variable that goes to a new folder you created called `my_stuff` in your home directory under the variable STUFF. 

```bash
cd $HOME
mkdir -p my_stuff
cd my_stuff
STUFF=$PWD
```

the set of lines above would create the folder `my_stuff` and point the `STUFF` variable to the path `$HOME/my_stuff`. You can print out what the variable is using the `echo` command and designating the `$` will read the variable's string value:

```bash
echo $STUFF
```

If you close the terminal session, the variable will disappear.

Now let's say you want use this variable within a shell script. Let's create the script `stuff.sh`:

```bash
#!/bin/bash

echo $STUFF
```

If we try to make it executable and run it:
```
$ ➜  chmod +x stuff.sh
$ ➜  ./stuff.sh 


```

We see that nothing is outputted. When you set a variable in this way, it only works with the current *parent* process. It cannot spawn any *child* process that can also use the variable. Thankfully, we can fix this by using the `export` command when setting the `STUFF` variable. Assuming you're still in the same directory, run the command:

```bash
export STUFF=$PWD 
```

Now if we re-run the script:
```bash
$ ➜  my_stuff ./stuff.sh 
/Users/username/my_stuff
```

### Common Environment Variables

When customizing our software environment, environment variables are used to control what the shell can see and use. The most common 3 variables you may need to modify are `PATH`, `CPATH`, and `LD_LIBRARY_PATH`. They all serve distinct rules to the shell environment.


### PATH variable

* `PATH` sets the directories you can run executables without specifying their entire path. For example, the text editor `nano` is (typically) located at `/usr/bin`. You can see this using the `which` command:

```
$ which nano
/usr/bin/nano
```

We can see all the executable sources from PATH by printing out the variable:


```
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
```

Hence, with this environment variable, all executables located in `/usr/local/bin`, `/usr/bin`, `/usr/local/sbin`, and `/usr/sbin`. Let's say you wanted to add our `stuff.sh` script to the `PATH` variable. You can do this by recursively appending the `$PATH` variable with our `$STUFF` variable we defined above:

```
$ export PATH=$PATH:$STUFF
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/Users/username/my_stuff
```

Now if we can change our directories, and access the `stuff.sh` script in any directory we're in, in that terminal 

```
$ cd $HOME
$ stuff.sh
/Users/username/my_stuff
```

## `LD_LIBRARY_PATH` & `CPATH`

Similary, the environment variables `LD_LIBRARY_PATH` and `CPATH` are important for setting up software variables.

`LD_LIBRARY_PATH` allows the OS to see what system libraries a program may need to run,. 

`CPATH` allows the OS to find header files for C-programming language codes.





 
