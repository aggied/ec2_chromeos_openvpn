EC2+OpenVPN+ChromeBooks
=======================

* ChromeBook is a great travel companion
* EC2 is a perfect service to launch a VPN endpoint on
* OpenVPN works both with ChromeBook and EC2
* This script makes all 3 work together to provide secure connection from
  locations you don't trust (coffee shops, hotels, airports... etc)
* Step by Step guide with screenshots can be found here:
   - http://porttwentytwo.blogspot.com/2013/03/openvpn-in-ec2-for-chromebooks-part-2.html

How is it done
==============

* Openvpn generates an .ovpn file which cannot be used by ChromeBooks.
* This script generates a .onc file which chromebooks can understand.
* This script will generate the certs and the openvpn server configuration required.
* The file is sent by mail to the user who can just download it, import it and connect to openvpn server immediately.


Detailed setup
==============

This script is specifically written for amazons EC2 instance using amazons linux distribution

* Step 1: Boot up an EC2 instance using Amazon's linux distribution
   + Use t1.micro if you want the cheapest solution
   + Use one of the latest versions of "Amazon Linux AMI" images, which is what
     this set of scripts is compatible with.
   + Whatever "security group" you pick, make sure you have 443,1193-1194 opened
     up for both TCP/UDP
* Step 2: Get the scripts...
```
wget https://github.com/aggied/ec2_chromeos_openvpn/archive/master.zip; unzip master.zip
```
* Step 3: Update vars.sh
   + This step is optional.
* Step 4: Run setup.sh <email_address>
   + When you get prompts, just press enter to select the default values
* Step 5: Read the "What to do on chromebooks" below to see how to import the ONC file.

Quick setup
===========
If you don't want to worry about customizing the certificate, just run this on
EC2 instance.

```
curl https://raw.github.com/royans/ec2_chromeos_openvpn/master/quicksetup.sh > quicksetup.sh;
chmod +x quicksetup.sh;
sudo ./quicksetup.sh email_address@blogofy.com
```

What to do on chromebooks
=========================

* Download the onc file sent by the script.
* Import ONC file from this page : chrome://net-internals/#chromeos
* At this point you should be able to see the openvpn listed in your connection settings.

Notes
=====

* This is the bare minimum configuration. There are a lot of things you could improve.
   + Enable PAM based login if you want to do password checks. This would be very helpful if you want to share this ONC file with multiple users.
   + However, if you do have multiple users, you should create a unique client cert for everyone.

* TLS-auth is disabled currently. I couldn't figure out how to enable it yet... I know it works, but its not there yet.

* I picked Amazon's EC2 instance with Amazon's distribution because thats the cheapest and most stable distribution on that platform (my personal opinion)
   + The scripts could be adapted to other platforms as well. Few things to think about
     - package manager may be different. 
     - I parse output from a few binaries to do things automatically... u may have to test them to make sure format changes don't break the script.

* Updated info here: https://github.com/royans/ec2_chromeos_openvpn/blob/master/README.md


Contributors
============
* Special thanks to the Ralf Robert Stebner who contributed the core script which did cert conversions and created ONC template:
  He can be found at: https://plus.google.com/114451000525400904136/
