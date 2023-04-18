
- Abuse of an application's behaviour to execute commands on the operating system.

- Biggest example of this is RCE.
	- Attacker can trick the application into executing a series of payloads without having direct access to the machine itself.


***Discovering Command Injection***

- Attacker can inject commands into unfiltered php code to run thier own code.

![[Pasted image 20230301225444.png]]

- Shell operators `;` `&` and `&&` can be used to cause command injection.

**Detecting Blind Command Injection**

- No input is produced from the application here; we need to use payloaads that cause some time delay in order to detect here.
			- `ping`, `sleep`, 
			- Application will hang for an amount of seconds related to how many pings we have specified.

- Can use `curl` to detect for command injection; allows us to deliver data to and from an application in our paylaod.

Ex: `curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami`


***Detecting Verbose (nonblind) command injection***

- Use a lot of common commands:

`whoami, ls, ping, sleep, nc, dir, timeout`

- `timeout` is a windows command used to invoke the application to hang, used when ping is not installed.

----
***Remediating Command Injection***

- Can be prevented by using non-dangerous functions or libraries with php or python (Flask).
		- Avoid usage of `Exec`, `Passthru`, `System` within PHP.

Example of application filtering malicious input:

![[Pasted image 20230301230350.png]]

- Input sanitization: sanitize special characters such as `>`, `&`, and `/`

How this looks:![[Pasted image 20230301230519.png]]


NOTE: These remediation techniques can still be defeated by  **bypassing filters**:

Ex:![[Pasted image 20230301230621.png]]

This payload will be executed in hexadecimal format, and will still be interpreted and produce the same result as another malicious implementation like `;whoami`.





