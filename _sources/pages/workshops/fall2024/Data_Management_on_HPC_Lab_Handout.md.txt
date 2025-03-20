# Intro to Data Management on HPC

## Checking data usage

If you want to see the size of files in a given directory, the `ls` command with the `-h`, `-l`, and `-a` flags will list all files in the directory in a human readable filesize:

```
ls -lah
```

### Using `du`

`du` reference: https://www.geeksforgeeks.org/du-command-linux/

To get a summary of data usage in your current directory, use 
```bash
du -sh
```

The `-s` flag gives you a summary, while the `-h` flag makes it human readable.

To get the 10 largest directories in a given directory, we can pipe `du` into the `sort` command. For the example below, we use it for our scratch directory:

```bash
du -a $SCRATCH | sort -n -r | head -n 10
```

> NOTE: This command can get slow if you have a lot of files. 


## Archive Data 

### Using `tar`

Tar reference: https://www.geeksforgeeks.org/tar-command-linux-examples/

To archive data using tar, it takes the following format:

```bash
tar -czf <filename>.tar.gz <list of directories>
```

For example, if we wanted to bundle our data from yesterday's lab, with the job data:

```bash
cd $SCRATCH
tar -czf Workshop_Fall2024_day2.tar.gz Workshop_Fall2024 jobs
```

Let's untar the data in a new folder:
```bash
cd $SCRATCH
mkdir -p new_data_folder
cd new_data_folder
cp ../Workshop_Fall2024_day2.tar.gz .
tar -xf Workshop_Fall2024_day2.tar.gz
```

*Exercise:* Try untarring the data to a specific folder say `~/scratch/test_oct12` using the `-C` flag. Look at the documentation for `tar` to figure this out.





## Transfer Data

### Using scp

#### Transfering individual file

Now transfer the tarball we created from Wendian to your home system (open a new terminal that is NOT logged into Wendian):
```bash
scp username@wendian.mines.edu:~/scratch/Workshop_Fall2024_day2.tar.gz . 
```

#### Transfering directory
You can also just transfer the directory directory using the recursive `-r` flag. Again make sure you have a terminal open that is NOT logged into Wendian:

```bash
scp -r username@wendian.mines.edu:~/scratch/new_data_folder . 
```

### Using rsync (Linux/macOS only)

Rsync is similar to scp, but will let transfers restart if they're cancelled. Here is a template for a typical rsync transfer:

```bash
rsync --rsh=ssh -rvP username@remote_host:/path/to/source /path/to/destination
```

The flag `–rsh=ssh` ensures rsync uses ssh. `-rvP` will recursively pull files from the directory (`-r`), with verbose output to the screen (`-v`) and allow for partial transfers (`-P`) in case an interruption or a restart. For example, to transfer the directory `new_data_folder` from Wendian to your local directory:

```bash
rsync --rsh=ssh -rvP username@wendian.mines.edu:~/scratch/new_data_folder .
```
If you want to purposely cancel it, press `CTRL+C` on your keyboard and cancel it. You can see on your local machine, by typing `ls`, that part of the file will still be there. If you did this with `scp`, you would not see a partial file. 

Now restart the transfer with the command above, you'll see it will pick up where it left off from the last cancellation.





### Using Graphical Applications

#### Filezilla

Go to https://filezilla-project.org/ and install Filezilla on your machine. Then open the application and fill in the information on the top:

* Host: [sftp://wendian.mines.edu](sftp://wendian.mines.edu)
* Username: Your Mines Username
* Password: Your Mines Password
* Port: 22

Try to transfer the same tarball down using the FTP client.


### Globus

Go to http://app.globus.org and login using your Colorado School of Mines Credentials. Try to pull down the tarball using this interface too.
 


```python

```
