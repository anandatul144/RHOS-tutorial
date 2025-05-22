## Chapter 1: Prerequisite — Basic Linux Commands and Applications

### Goals

* Understand basic Linux shell commands
* Get comfortable with navigation, file operations, and process management

---

### Navigating Directories

These are the first commands you should feel confident using. Open your terminal and try each of the following:

```bash
pwd       # Print current directory
ls        # List files in the directory
cd ~      # Go to your home directory
cd /etc   # Go to /etc directory
cd -      # Go back to the last directory
```

Try creating a test directory and exploring it:

```bash
mkdir test_folder
cd test_folder
pwd
```

---

### File Operations

Learn how to create and manipulate files and directories:

```bash
touch hello.txt         # Create a new empty file
mkdir my_folder         # Create a new directory
cp hello.txt my_folder/ # Copy file to directory
mv hello.txt greetings.txt # Rename a file
rm greetings.txt        # Delete a file
```

Try editing and viewing files:

```bash
nano hello.txt         # Edit a file with Nano editor
cat hello.txt          # Display file content
less hello.txt         # Scrollable view
head -n 5 hello.txt    # Show first 5 lines
tail -n 5 hello.txt    # Show last 5 lines
```

---

### File Permissions

See and change who can read/write/execute a file:

```bash
ls -l hello.txt       # Show permissions
chmod 444 hello.txt   # Read-only for everyone
chmod u+x hello.txt   # Add execute permission for owner
chown root:root hello.txt  # Change file owner (as root)
```

---

### Package Management

Install, update, and remove software using package managers. Use the one for your distribution:

* Fedora / RHEL:

```bash
dnf install cowsay
yum update
```

* Debian / Ubuntu:

```bash
apt update
apt install cowsay
```

---

### Networking Tools

Check network connectivity and download files:

```bash
ping -c 4 google.com      # Test internet connection
curl https://example.com  # Fetch page
wget https://example.com  # Download file
ip a                     # Show IP addresses
ss -tuln                 # Show listening ports
```

---

### Process Monitoring

Monitor and control what's running on your system:

```bash
ps aux          # List all running processes
top             # Real-time process view
htop            # Advanced interactive view (may need to install)
kill <PID>      # Terminate a process
systemctl status sshd  # Check service status
```

---

### Assignment: Try It Yourself

#### Objective

Practice everything you've learned so far.

#### Instructions

1. Open a terminal and navigate to your home directory:

   ```bash
   cd ~
   ```

2. Create a new directory:

   ```bash
   mkdir rhos_lab
   cd rhos_lab
   ```

3. Create a new file and add your name to it:

   ```bash
   echo "YourNameHere" > hello.txt
   ```

4. Make the file read-only:

   ```bash
   chmod 444 hello.txt
   ```

5. View permissions:

   ```bash
   ls -l hello.txt
   ```

#### Verification

* The output of `ls -l hello.txt` should show `-r--r--r--`.
* Running `cat hello.txt` should display your name.
* Attempting to edit `hello.txt` with `nano` should result in a permission denied message.

This concludes the foundational chapter. Proceed to the next chapter only after completing all exercises successfully.
