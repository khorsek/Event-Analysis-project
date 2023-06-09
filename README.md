# Event-Analysis Project

# This is the lab writeup of the time I had to analyze a PCAP file that was recorded while a home network was "experiencing significant events". I'll try to answer a few questions and determine what could have happened during this session. 

# This lab was done a few months ago and I'm just importing this info to document on GitHub.

# The first thing I do is open up my Windows virtual machine that I have for training purposes and look for my PCAP file. Once I find it on the desktop, I open it up in Network Miner, an open source application for exploratory analysis and visualization of large network data. 

# Once it loads, I get my first view of all the information for me to parse through.
<img src="images\1.PNG">

# Looking at the image, it has about 1695 parameters, 83 hosts, 62 files, 16 images, and so on.

# The first thing that I think to check is to see how long exactly, the entire session capture lasted. So I open up the sessions tab and compare the start time of the first entry to the very last one:
<img src="images\2.PNG">

# So the session began capturing at about 9:30 PM in 12 hour time and lasted until 9:37 PM, a total of 7 minutes worth of captured data.Now that I’ve determined how long the session capture lasted, I try to determine how many packets were “captured” or received as it’s known in Network Miner. 
# To get the number of packet’s captured, I moved back to the “hosts” tab and sorted each host by the number of received packets(descending). As seen in the figure 3 below, I circled the area where it’s shown how many packets are captured and this way, I went down each host and calculated the sum of all these packets until I got the total amount of packets captured.
<img src="images\3.PNG">
<img src="images\4.PNG">

# After going down each of the highlighted hosts and calculating the total amount of packets received, the number comes out to ~2325 packets and ~768,933 bytes captured over the entire 7 minute session which is surely a handful. Now that I’ve determined exactly how many packets and bytes were captured, I move on to try to see which protocols(or application layers) were observed during the entire 7 minute capture. 
# To do this, I move on over to the “Sessions” tab and immediately, you can see each of the Sessions showing which protocol was observed for them. If I sort by Protocol, I can see a number of different protocols that were observed, those being: FTPControl, HTTP, Oscar, and SSL. You can see a glimpse in the figure below:
<img src="images\5.PNG">

# Since I was a little unsure of the significance of each observed network protocol, I decided to do a little research into them. FTPcontrol is a “network protocol for transmitting files between computers over TCP/IP connections” (Kerner & Burke, 2021) OSCAR is “AOL’s instant messaging and presence protocol for Open System for Communication in Realtime” (Oscar Protocol) Finally, SSL is “an encryption-based internet security protocol…for the purpose of ensuring privacy, authentication, and data integrity…” (What is SSL (secure sockets layer)? | cloudflare)

# Now that we’ve taken a look at the amount of packets and bytes received, as well as which kind of network protocols were observed, I decide to analyze some of the data we’ve gotten so far and see when the “bulk of the data” got transmitted. To do this, I took a look at the excel data regarding the packets and bytes and noticed a huge jump in regard to the number of packets being received and decided to cut it off at 92 packets
<img src="images\6.PNG">

# After filtering out each of the IP addresses and checking their session time, I’ve determined that roughly, the bulk of the data got transmitted within the initial 4 minutes of the 7 minutes captured.
<img src="images\7.PNG">

# By checking the files tab and filtering for some of the IP’s, I can see numerous .gif and .jpg files which are most likely responsible for the spike in transmission data as seen below for one of them:
<img src="images\8.PNG">

# Looking in the DNS tab, I can see numerous entries for “internet.aol.com”, all of which go to the same server address of “homeportal.gateway.2wire.net” which makes me think that this is the internet service provider whose websites are being accessed.
<img src="images\9.PNG">

# I took a look in the credentials tab but it doesn’t seem to contain any accounts for the AOL internet service as even the IP addresses don’t match with the ones in the DNS tab.
<img src="images\10.PNG">

# As pictured in figure 11 below, the name of the host computer is “Kaufman Upstairs, KAUFMANUPSTAIRS”, with its IP address being 172.16.1.35:
<img src="images\11.PNG">

# The host PC, as hinted at by the icon that the IP address is attached to is using the Windows operating system. The local network seems to contain 97 outgoing sessions, some of the device names being “linux -wlan.org”, “faculty.business.utsa.edu”, and “msn.com” for example. It does look like some other computers are being accessed as seen in figure 12 below, you can see that the server is running on windows just like the client which is “KaufmanUpstairs”:
<img src="images\12.PNG">

# Another device that can be seen being accessed is a linux device as been below:
<img src="images\13.PNG">

# In regards to what kind of story the capture file tells, I think it’s possible that an attacker attempted to use an “ARP spoofing” or “ARP poisoning” attack that would allow him to intercept communication between network devices, in this case, between the HOST Computer and the several websites that he was browsing on. 
# As evidenced by the credentials tab, it looks like he may have attempted to harvest credentials that access the hosts’ aol internet service provider account, his yahoo mail (since he uses his PC to read email), perhaps his msn account as well, and finally, his utsa faculty account credentials. I do think the events that have happened can be concerning and this was a malicious man-in-the-middle attack.

# Now that we’ve analyzed the PCAP file in network miner, it’s time to attempt the same in Snort. According to Snort’s own website, it’s the “foremost Open Source Intrusion Prevention System (IPS) in the world. Snort IPS uses a series of rules that help define malicious network activity…useful for network traffic debugging…can be used as a full-blown network intrusion prevention system” (Snort 3 is available!)

# After opening up Snort, I got an error and after a little bit of troubleshooting, realized that the snort.config file comes with a plethora of bugs that you have to fix, as well as many PATH corrections. After a while of bugfixing and testing, I finally got Snort to successfully validate the configuration and continue as seen below:
<img src="images\14.PNG">

# After running the PCAP file in snort, it gave me the following information regarding any alerts, which I couldn’t really discern and then realized that it had made a log tcpdump.
<img src="images\15.PNG">

# Conclusion: After opening up the log in Wireshark and looking over the entire file, I can’t seem to find any evidence of malicious activity. When I first plugged the PCAP file into Network Miner, it said that it thought an ARP poisoning attack was likely, but I can’t seem to find any evidence of that in Wireshark.

# <center>End of Lab.

# <center>Bibliography            

# Kerner, S. M., & Burke, J. (2021, May 6). What is FTP? file transfer protocol explained. Networking. Retrieved March 1, 2023, from https://www.techtarget.com/searchnetworking/definition/File-Transfer-Protocol-FTP
# Oscar Protocol. OSCAR protocol - Academic Kids. (n.d.). Retrieved March 1, 2023, from http://academickids.com/encyclopedia/index.php/OSCAR_protocol
# What is SSL (secure sockets layer)? | cloudflare. (n.d.). Retrieved March 1, 2023, from https://www.cloudflare.com/learning/ssl/what-is-ssl/
# Snort 3 is available! Snort. (n.d.). Retrieved March 1, 2023, from https://www.snort.org/

