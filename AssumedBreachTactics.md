# Egress-Assess

Egress-Assess is a tool used to test egress data detection capabilities. Typical use case for Egress-Assess is to copy this tool in two locations. One location will act as the server, the other will act as the client. Egress-Assess can send data over FTP, HTTP, and HTTPS. 

To extract data over FTP, you would first start Egress-Assess’s FTP server by selecting “--server ftp” and providing a username and password to use: 

    ./Egress-Assess.py --server ftp --username testuser --password pass123 

Now, to have the client connect and send data to the FTP server, you could run... 

    ./Egress-Assess.py --client ftp --username testuser --password pass123 --ip 192.168.63.149 --datatype ssn 

Also, you can set up Egress-Assess to act as a web server by running.... 

    ./Egress-Assess.py --server https 

Then, to send data to the FTP server, and to specifically send 15 megs of credit card data, run the following command... 

    ./Egress-Assess.py --client https --data-size 15 --ip 192.168.63.149 --datatype cc 

Reference: https://github.com/RedSiege/Egress-Assess

WinPWN - saves the information under user profile, Need to cleanup after done with system or it will leave that content behind 

- \\domainname\sysvol

- Look for system32\unattend.xml files for credentials
