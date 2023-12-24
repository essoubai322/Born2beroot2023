##****NOT: "IF YOU ENCOUNTER ERRORS SUCH AS “NO COMMANDS”, TRY AGAIN WITH THE “SUDO” PARAMETER, THE PROBLEM WILL MOST LIKELY BE SOLVED.****
**Once the SDAs are configured,
**

1-) “su” -->Log in to the root account by typing (the first password we set when setting up the system, which will ask for the root password)

2-) “apt install sudo” --> We type and install the sudo package (sudo gives users root privileges)

3-) “adduser <user> sudo” --> Adds the specified user to the sudo group. NOTE: Check if the user is added to the group with “getent group sudo”!

4-) We reboot the system with the “reboot” command.

5-) After logging in to the user added to the group, let’s check again with “sudo -v”.

6-) “sudo apt update” --> Let’s write and update the package lists.

7-) “sudo visudo -f /etc/sudoers.d/blabla” --> Opening sudo files with “visudo” creates a safer structure. We created a new file named blabla in the specified directory and will add the codes below.

@-) Defaults passwd_tries=3 // With sudo, the maximum number of password attempts is set to 3 (by default it is 3)

@-) Defaults badpass_message="bla bla bla bla bla bla" // Your error message that will be displayed after incorrect password attempts

@-) Defaults logfile="/var/log/sudo/blabla" // Stores all used sudo commands in the specified file

@-) Defaults log_input,log_output // It is used to keep logs of inputs and outputs.

@-) Defaults iolog_dir="/var/log/sudo" // Archives log_input and log_output events to the specified directory.

@-) Defaults requiretty // Enforces TTY mode.

@-) Defaults secure_path="pdf'ye bak" // To limit the directories used by sudo.

8-) We reboot the system with the “reboot” command.

9-) “sudo apt install openssh-server” --> Installs the openssh-server package (Openssh provides a secure channel over an unsecured network from outside)

10-) “sudo nano /etc/ssh/sshd_config” --> SSH Configurations section.

@-) Change “#Port 22” to “Port 4242” without quotes

@-) Change the “#PermitRootLogin prohibit-password” section to “PermitRootLogin no”. NOTE: You will do it without the quotes and # signs.

11-) We reboot the system with the “reboot” command.

12-) “sudo service ssh status” --> You can view the ssh status by typing this command.

13-) “sudo apt install ufw” --> It stands for “uncomplicated firewall”, which means it is an uncomplicated firewall (: you install that package.

14-) “sudo ufw enable” --> We activate our firewall.

15-) “sudo ufw allow 4242” --> We add a new rule to our firewall for port 4242 and allow it.

16-) “sudo ufw status” --> We check the status of our firewall.

17-) We shut down the system with the “poweroff” command.



THIS IS IMPORTANT: IN ORDER TO MAKE AN SSH CONNECTION, WE NEED TO PERFORM PORT FORWARDING THROUGH THE VM.



-Right click on the virtual server we have set up in the VM interface, go to the network section in the settings section, then click on the Bpoint routing section under the advanced menu.

On the right side of the new window that opens, there is a green new connection rule button, let’s click it once. Later

-NAME Section: SSH

-PROTOCOL Section: TCP

-HOME MACHINE IP Section: 127.0.0.1

-To MAIN MACHINE B.N Part: 4242

-GUEST IP Section: 10.0.2.15

-GUEST To B.N Section: 4242



You can edit it as follows and press OK below.



--------------------------------------------------------------------------------------------------------------------------------



18-) "sudo nano /etc/login.defs"			--> We open the file with Nano and search in it (NOTE: DO NOT WORRY ON THE VIRTUAL SERVER ITSELF AS WE CAN NOW ASSIST AN SSH CONNECTION THROUGH THE TERMINAL) :)

	@-) PASS_MAX_DAYS	99999 --> PASS_MAX_DAYS 30 //We specified that the password will expire every 30 days

	@-) PASS_MIN_DAYS	0 --> PASS_MIN_DAYS	2 // We stated that after changing the password, it will be changeable again at least 2 days later.

	@-) PASS_WARN_AGE   7 //We specified that the user should receive a warning 7 days before his/her password expires.

19-) "sudo apt install libpam-pwquality"	--> We install the libpam-pwquality package to increase the security of passwords.

20-) "sudo nano /etc/pam.d/common-password"	--> common-password The document is opened and searched: "password		requisite						pam_pwquality.so retry=3"

	-retry=3'The following is added by leaving a space after:

		@-) minlen=10					// Minimum length 10

		--

		@-) ucredit=-1 dcredit=-1		// It must contain at least one uppercase and one numeric character.

		--

		@-) maxrepeat=3					// A maximum of 3 consecutive characters can be used.

		--

		@-) reject_username				// It should not contain the username.

		--

		@-) difok=7						// Requirement for 7 different characters that the old password does not contain

		--

		@-) enforce_for_root			// We also made the rules valid for root

		--

		@@-) FINAL STATE: password		requisite						pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root

-------------

NOW, TO TEST OUR NEW CONFIGURATIONS, WE WILL CHANGE THE PASSWORDS OF THE OTHER USER WE OPEN ON BEHALF DURING THE ROOT AND INSTALLATION.

21-) "sudo addgroup user42"					--> "user42" We created a new group called.

22-) "sudo adduser kullanıcı_adı user42"	--> user42 We added the specified user to the group.

23-) "sudo passwd kullanıcı_adı"			--> Let's change our passwords.

24-) "sudo crontab -u root -e"				--> Crontab can be thought of as a timed file execution system. bottom line "*/10 * * * * sh /path/to/script" change or add.


