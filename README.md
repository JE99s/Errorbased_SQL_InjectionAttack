<h1>SQL Inection Attack On Mutillidae User Info Page</h1>
Simulated SQL Injection attack that is a SQL error-based injection which involves the Mutillidae User Info page as the victim site.
<h2>Description</h2>
<p>Project consists of a simulated SQL Injection attack that is a SQL error-based injection which involves the Mutillidae User Info page as the victim site. </p>
<br/>
<h2>Utilities Used</h2>

- <b>Text editor</b>
- <b>Web browser</b>
- <b>VMware Hypervisor</b>
<h2>Environments Used</h2>

- <b>Kali Linux</b>
- <b>Metasploitable 2 (MS2) Vulnerable Machine</b>
<h2>Preparation</h2>
Firstly, I want to power my ESXi machine on my VMware Workstation Pro application. Once it completely boots up, I use the provided URL on the homepage of the ESXi machine. I want to open my Kali Linux VM from Lab 00. I ensure that the network adapter is in “Bridged” mode.
<br/>
<img src="https://i.imgur.com/wgr3cN1.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
My MS2 virtual machine (VM) and Kali Linux VM are in the same network. I’m also going to use Firefox on my Kali VM to connect to my MS2 web server using its IP address to further ensure that there is connectivity between the two machines.
<br/>
<img src="https://i.imgur.com/yScwa2c.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
<h3>Repair Mutillidae Configuration File</h3>
Next, I navigate to my MS2 VM. When it comes to a default installation of MS2 the database name is set to “metasploit” by default inside the machine’s mutillidae configuration file. For my attack to be successful, I need to change “metasploit” to “owasp10”. So, on my MS2 VM, I execute <i>sudo nano /var/www/mutillidae/config.inc</i> to modify the configuration file, running the command as a root user. I enter the password when prompted and I’m in the configuration file.
At the last line where it begins with “$dbname” I set it equal to “owasp10” instead of “metasploit”. After that, I just press <b>CTRL+X</b> to exit, type <b>Y</b> to save the file, and press <b>Enter</b> to confirm and exit the file.
<br/>
<img src="https://i.imgur.com/NummpFf.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
Back on my Kali Linux VM, in the same MS2 web server, I will click the Mutillidae link, and it takes me to the Mutillidae website page. I click <b>OWASP Top 10 > A1 - Injection > SQLi - Extract Data > User Info</b>. As shown in the error-based SQL injection video gracefully made by NFE Systems Ltd. To get into a SQL injection you should start by entering special or reserved characters in the user name field to produce SQL syntax errors to exploit. Following along with the video instructions I made my own list of special reserved characters in a <b>.txt</b> document, as shown below.
<br/>
<img src="https://i.imgur.com/zXARBmZ.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
I enter all the special reserved characters on <b>line 1</b> of the <b>.txt</b> document into the Name field on the User Info page via copy and paste. I click the <b>View Account Details</b> button to submit the credentials.
<br/>
<img src="https://i.imgur.com/mQ0zjCS.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
I received an error message detailing the error in executing query. There is an error in SQL syntax and the message tells me that this database or server is running in MySQL. There’s also the SELECT statement set query that’s running in the background. I want to copy all contents inside the <b>Diagnostic Information</b> field and paste that into another <b>.txt</b> document.
<br/>
<img src="https://i.imgur.com/Foa77GA.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
Notice the SELECT statement. This is what happens inside the logs of the server when users log in.  Inside the bottom Mousepad window, the statement underlined in red is my input from earlier which is provided in the Mousepad document above as well. That is what I have control of, and the SELECT statement is what the website does to retrieve data from its SQL server. For this injection, I need to get this SELECT statement to always be true. So, on <b>line 3</b> I type ‘ or “a”=”a”.
<br/>
<img src="https://i.imgur.com/5lLiyNP.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
Now the statement tells us that <b>Select *</b> (all columns) <b>From accounts where username</b> is equal to ‘(nothing) or a equals a. We know that “a” is always going to be equal to “a”, so that statement provides a true statement that I’m looking for. The bit at the bottom with the single quotes and password equals ‘ ‘ needs to be commented out.
<br/>
<img src="https://i.imgur.com/D2Md9e0.png" height="75%" width="75%" alt="Linux command line steps"/>
<br />
I put a # (hash) at the end of <b>line 3</b> which will comment out the rest of the line giving me what I’m looking for. Now, I can try to attempt the attack again by this time entering the contents of <b>line 3</b> back into the <b>Name</b> field on the login page provided by the User Info page. I press the <b>View Account Details</b> button and now I can see 16 records and their respective usernames, passwords, and signatures all dumped on the page.
<br/>
<img src="https://i.imgur.com/93oufVv.png" height="80%" width="80%" alt="Linux command line steps"/>
<br />
