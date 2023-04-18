# Linux Fundamentals (pt. 3)

- First introduces us to linux text editors such as *nano* and *Vim*, focuses on nano here.

- Vim is a much more advanced text editor that is customisable and uses syntax highlighting. Vim works on all terminals.

**Downloading Files**

- Use `wget` to download files from the web via HTTP - use `wget <address of website here>` 

**Securely Copying Files**

- Use secure copy `scp` to securely copy files. Different from the regular `cp` command in that it allows you to transfer files between two computers using the ssh protocol.

   - `scp` allows you to copy files and directories from your current system to a remote system and vice versa.

Ex: If we know the ip address, user, name of file, and the name of file we wish to store the file under, we can secure copy to a remote system like this:

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

**Serving files from your host using python**

- Python supports a module called *httpserver* which turns your computer into a quick and easy web server that can be downloaded onto other machines using curl and wget.

Start the server: `python3 -m http.server`

Then we can use `wget http://127.0.0.1:8000/file 
to download a file named 'file' from the locally hosted web server.

**Processes**

- You can predominantly view processes using the `ps` command. This provides a list of the running processes on our user's session, as well as some additional information such as their status codes, session that is running it, CPU usage.

- Add the `aux` option to `ps` to see processes run by other users along with system processes.

- `top` gives real-time statistics about processes running on your system in a dynamic review that is refreshed every ten seconds.

- Use `kill` to end processes. Can use signals such as `SIGTERM, SIGKILL, SIGSTOP`. 

- Operating systems use *namespaces* which split up resources available on the computer into processes. Namespaces have security benefits in that they isolate processes from another; only those in the same namespace can see each other.

- Critical processes often start on the boot of a system. In order to tell the system to start a service such as apache web server to boot manually, we would use `systemctl` which allows us to interact with the `systemd` process/daemon.

Ex: `systemctl start apache2`

- Can do `start, stop, enable, disable` with `systemctl`.

**Backgrounding and Foregrounding**

- Add the '&' operator to a command to background it. 
   - Useful for copying files or running a webpage in the background.

- Can do the same for scripts by pressing ctrl + z on our keyboard to essentially 'pause' the execution of a script.

- Use `fg` to bring the process back to the foreground, where it can be back into use again.

**Crontabs**

- The `cron` process can be interacted with using crontabs that require 6 values:
   - MIN
   - HOUR
   - DOM - day of month
   - MON - month of year
   - DOW - day of week
   - CMD - command that will be executed

- Can edit crontabs by using `crontab -e`

**Packages and Software Repos**

- When developers want to submit software, they submit it to the `apt` repository where their programs and tools will be released into the wild.

- Can add community repositories to extend the capabilities of your OS: `add-apt-repository`

- Benefits of installing software through apt is that whenever the system is updated, the repository that contains the pieces of software we add gets checked for updates.

**Logs in Linux**

- Logs are located in the `/var/log` directory; the files and folders contain logging information for applications and services running on the system.

- Looking at these services and logs is a good way to monitor the health of a system.

