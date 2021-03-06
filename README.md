# ZeekDoc
 
 <p align="center">
  <img src=/Documentation/zeekLogo.png width=100>
 </p>
 

## Bro/Zeek 

### What is Zeek?

Zeek is a network analysis and IDS tool. It is an alternative to various other tools such as Snort, Suricata and OSSEC. While Snort,Suricata and Zeek to quite an extent are majorly used to perform Netwprk based IDS(NIDS) tools like OSSEC are used for Host based IDS(HIDS- such as antivirus, firewall etc).further more most IDS are generally broadly categorized into two types. The signature type or the anomaly detection type, are the two primary detection techniques. The signature type's strategy is to analyse the network traffic for predefined patterns or signatures and then raises alerts when detected where as the anomaly detection type works on listening to the network carefully and detecting any strange behaviour. The chances of a false positive are much higher in anomaly based detection but also conversely leaves room for newer attacks to happen without the signature based IDS even knowing. 
<br> Read more in the documentation : https://docs.zeek.org/en/current/intro/index.html#overview

### Why Zeek?

Unlike many other IDS such as Snort and Suricata, Zeek is a hybrid of both anomaly and rule based techniques. Zeek engine structure is indeed very efficient and useful as it creates incoming traffic into a series of events which can be either treated or left as is. An event could be anything like a a new connection , a new ssh connection or anything. The real power of Zeek comes with the Policy Script interpreter. This engine has its own scripting language (Bro/Zeek scripts eg anomaly.zeek) which comes with its own custom made data types and data structures tailored for the use of dealing with network data. Hence Zeek is able to give huge control and power to the user to be able to analyse the data in ways which would have proven very difficult before making it a decent choice to be used as an HIDS too. The user can now generate custom rules and notifications for any particular scenario and so doesent have to rely on predefined rules. Zeek can mimic anything that the conventional rules based IDS such as Snort and Suricata can do with the added advantage of customisability. A network activity in one subnet may be deemed illegal while not in another, this is where Zeek plays its trump card.
<br><br>
Now Zeek is not just merely an IDS, due to its powerful scripting nature it can be used to perform various complex tasks such as incident response, forensics, file extraction, and hashing among other capabilities. Due to its Policy engine we can provide custom scripts to perform certain operations on incoming traffic at any time. Zeek is a logging based IDS, that means it provides log files with well documented extraction data from the network activity which can be used for forensics in the term that some activity occured. The user can also write custom scripts to log additional computed information or specific parts of the connection based on the given script.
<br> Further more features as per official documentation : https://docs.zeek.org/en/current/intro/index.html#features
<br><br>

Further useful links to know more about zeek:
<br>https://zeek.org/
<br>https://www.admin-magazine.com/Archive/2014/24/Network-analysis-with-the-Bro-Network-Security-Monitor
<br>https://bricata.com/blog/what-is-bro-ids/ (very informative website with further links)
<br>http://www.iraj.in/journal/journal_file/journal_pdf/3-27-139087836726-32.pdf <br>a research paper that displays a brief comparison and uses of various IDS


## Zeek Architecture

Basic zeek architecture can be seen in the following diagram:
<p align="center">
  <img src="/Documentation/zeekArchitecture1.PNG" width=400>
 </p>
Zeek is single threaded, hence very often we set up a cluster mechanism where incoming traffic is split into multiple nodes. While zeek can still be used on the personal computer with the single device config it also has a cluster configuration. The cluster devices act as worker threads and all connected to the manager that acts as an interface for the user. In large networks it is absolutely neccessary to do this as the incoming traffic is huge and broad hence need more compute power and fast accessible memory. Pattern detection becomes difficult when the incoming traffic is scattered over a large number of interleaved packets from various user requests/connections hence more the memory the better. The following diagram shows the architecture as explained above.<br>
<p align=center>
  <img src="/Documentation/zeekarchitecture.png">
</p>
<br>for more understanding on the diagram visit : https://docs.zeek.org/en/current/cluster/index.html

<br>Cluster config can be done natively or using PF_RING which gives added speed to packet capturing.
cluster config link : https://docs.zeek.org/en/current/configuration/index.html


## Try Zeek Online 

Zeek has an interactive web editor to try and learn. It goes a through a wide range of examples to give a good feel on the language and its native data types and structures.
<br> link : https://try.bro.org

## Zeek Installation

There are multiple ways to install/use zeek for analysis.


### Linux:
The ways i encountered during my experience were either to install the binaries provided on the zeek documentation or make it from source from the github repo. Follow the fallowing steps to install instantly:
#### Dependencies:
dependencies : https://docs.zeek.org/en/current/install/install.html
##### RPM/RedHat
install dependencies
```
sudo yum install cmake make gcc gcc-c++ flex bison libpcap-devel openssl-devel python-devel swig zlib-devel
```
On RHEL/CentOS 6/7, you can install and activate a devtoolset to get access to recent GCC versions. You will also have to install and activate CMake 3. For example:
```
sudo yum install cmake3 devtoolset-8
scl enable devtoolset-7 bash
```
##### DEB/Debian
install all the dependencies:
```
sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig zlib1g-dev
```
#### Making From Source

clone the repo files as below
```
git clone --recursive https://github.com/zeek/zeek
```
get into the folder and run the following commands
```
./configure
make
make install
```
Once completed zeek would have created a folder at /usr/local/zeek which will contain sub folders with all executables and other important data.
#### Pre Built Binary Release Packages

bundled source packages : https://zeek.org/get-zeek/
latest dev versions:  https://github.com/zeek

###  Post build generation (either from src or pre built binaries)

After these steps, either temporarily export the zeek binaries to path every time (not recommended obviously) or add the export command to the bash/profile file so that its executed at the beginning of the bash session.
```
export PATH=/usr/local/zeek/bin:$PATH
```
Another way is to add manually a copy of the bin folder in zeek to the /usr/bin. This will add all the zeek binaries as system binaries.
```
cp /usr/local/zeel/bin/* /usr/bin 
```
Note: while installing, do not forget to use sudo if not in root, as many of these commands require to create directories and hence require enhanced permission.
### Windows :

Theres no official build for windows as such, since its an open source software. One way to install it would be use the WSL/WSL2. Installation on WSL/WSL2 will follow the same procedure as the Linux way. 
<br> Note: WSL/WSL2 commands like make/cmake run much slower and the installation might take longer than usual.

### Security Onion VM/OS

Another third way that comes as a package deal with many other software that help with zeek analysis is to install the SecurityOnion Linux Distro. It comes pre installed and configured with zeek and the elastic kibana stack that can be used to perform some data visualisation. SO (Security Onion) can be installed on a vm with configuration to listen on the host, the following youtube video will help with the config and installation of the VM.
<br>https://www.youtube.com/watch?v=jRoQUVY-2Ic
<br>Note: to run it smoothly, it is a very resource draining process hence minimum requirement would be to use it with atleast 8gb ram allocated to it. 4GB ram will work too for simple programs at a beginner stage, but 8gb is optimal for deployment or complex programs.

### Mac :
Personally I do not have experience working on a mac but i will attach the offcial documentation link below:
<br>https://docs.zeek.org/en/current/install/install.html

### ZeekCTL

The Zeek Control Shell is an interactive shell used to manage installations on a single system. In the examples below, $PREFIX is used to reference the Zeek installation root directory, which by default is /usr/local/zeek if you install from source.
<br>Now start the ZeekControl shell like:
```
zeekctl
```
Since this is the first-time use of the shell, perform an initial installation of the ZeekControl configuration:
```
[ZeekControl] > install
```
Then start up a Zeek instance:
```
[ZeekControl] > start
```
If there are errors while trying to start the Zeek instance, you can can view the details with the diag command. If started successfully, the Zeek instance will begin analyzing traffic according to a default policy and output the results in $PREFIX/logs. 
<br><br>
You can leave it running for now, but to stop this Zeek instance you would do:
```
[ZeekControl] > stop
```
<br> Follow config steps as per your choice here : https://docs.zeek.org/en/current/quickstart/index.html#managing-zeek-with-zeekcontrol
<br> further documentation on make :https://github.com/zeek/zeekctl


## Zeek command line tools

The zeek command can be used in multiple ways. Some usual commands used are as below: 
```
zeek -i enp0s3
```
```
zeek -i enp0s8 
```
```
zeek -i any
```
The -i helps us choose an interface. Interfaces may include the wifi, ethernet, bluetooth etc. Also VMs may have an additional source of having to listen to the host. The above commands then populate the current working directory with the logs. To exit the current directory use ctrl c.<br>
Note : 
<br>in all cases accessing sockets/ports info is a privileged command hence use sudo.
<br>most common new system of labeling for internet interfaces have changed from eth0 to enp0s for linux system. To understand more view the below link -
<br>https://unix.stackexchange.com/questions/134483/why-is-my-ethernet-interface-called-enp0s10-instead-of-eth0
<br> zeek can also be used with pre recorded packet captures. Various tools to perform packet capture are : wireshark and tcpdump. the following example stores live network data into mypackets.trace
```
sudo tcpdump -i en0 -s 0 -w mypackets.trace
```
where en0 is the interface.
```
zeek -r mypackets.trace
```
This command will do the same as running it on and interface and fill the current directory with the logs.
<br><br>
Now to run custom scripts on the current interface use:
```
zeek <options> <scripts...>
```
To see how its use follow to the next section.

### Zeek examples

Now we shall see couple examples to support my explanation of the zeek event driven model, logging struture and other such things that will help you get started with Zeek if you already havent tried it out on their official web tutorial. 
<p align="center">
  <img src= "/Documentation/test1.png" width=800>
</p>
The example shown in the image above shows three events. The init which is used as an initialisation to what the code has to do. The done event is executed when the code is terminating performing last set of commands at the end. This event can be used to do any sort of statistical analysis on data that must have been collected during the run of the script. Any other event is used to perform tasks or analyse, in this example we use the event on connection that has a parameter c which is a record ( zeek datatype similar to c structures). This record consists of various other nested records within it that hold the information of every aspect of the connection. In this example I have printed out a set of values from all of it. You can even print out the complete record which is generally very huge per connection. Further more we define a global variable of the type count( zeek datatype) that will be used to restrict and print the information only for the first 10 connections as shown by the if condition.
<p align="center">
  <img src= "/Documentation/test1run.PNG" width=800>
</p>

Run the following code and get information on the screen as shown above.
<br> let it run for a while and you will see logs as below:
<p align="center">
  <img src= "/Documentation/zeeklogs.PNG" width=800>
</p>

### ZeekCut
Zeek cut is a tool provided by zeek it is very helpful in dealing with quick manual analysis on logs. It efficiently helps you slice out information from the logs you might want to view.

eg  :
```
cat conn.log | zeek-cut id.orig_h id.orig_p id.resp_h duration
```
<p align="center">
  <img src= "/Documentation/zeekcut.PNG" width=800>
</p>

```
cat conn.log |zeek-cut -d ts uid host uri < conn.log
````
<p align="center">
  <img src= "/Documentation/zeekcut2.PNG" width=800 >
</p>

### Zeek Events

### Simple Portscan in Zeek

### Machine Learning and Cybersecurity

Machine learning has become a vital technology for cybersecurity. Machine learning preemptively stamps out cyber threats and bolsters security infrastructure through pattern detection, real-time cyber crime mapping and thorough penetration testing. A subset of artificial intelligence, machine learning uses algorithms born of previous datasets and statistical analysis to make assumptions about a computer's behavior. The computer can then adjust its actions — and even perform functions for which it hasn’t been explicitly programmed.
<br><br>
And it's been a boon to cybersecurity. With its ability to sort through millions of files and identify potentially hazardous ones, machine learning is increasingly being used to uncover threats and automatically squash them before they can wreak havoc. Machine learning algorithms come in many shapes and forms, but most of them perform one of three tasks:
<p align="center">
  <img src="/Documentation/mlAndCybersec.PNG" width=600>
</p>
<br>
To read more about various companies and how they tackle attacks I'd suggest this article : https://builtin.com/artificial-intelligence/machine-learning-cybersecurity
<br>
Some very interesting directed usages of ml can be found here : https://www.exabeam.com/information-security/machine-learning-for-cybersecurity/
<br>
Another link that will be helpful to give some perspective from a firm :https://acodez.in/machine-learning-cyber-security/
<br>

### Zeek with Python

Although Zeek is a scripting language itself often we will want to use it with python to perform as said above tasks like machine learning. Zeek scripts are used to perform lightweight computation on the go cause it needs to work at the speed of the incoming network at most times, while also performing resource draining tasks such as logging. Hence to perform heavy algorithms and data crunching tasks such as machine learning we will use python, which is loaded with tons of packages to perform such tasks.
<br><br>
The manual way of runnning these scripts on the logs would be to file read the logs and then use them in any given format. That turn out to be very annoying as logs have lot of meta data along side and hence reading the particular data is quite painful. Hence instead an alternative it to use the tool ZeekCut as mentioned above to extract the specific info you want and write it to a csv file. Why this is helpful is because then we can use the pandas package to read csv files into dataframes very easily. While this is better than the previous option obviously, its quite a pain by itself yet. This is where a new python package -ZAT made by open source developers specifically for Zeek comes into the picture.

### ZAT

Stands for Zeek Analysis Tools, developed by a company called SuperCowPowers which contribute to open source python community. This packages repo on github provides tons of functionality and modules all which can be used to integrate zeek with python much easier than ever. They have created several functions that help us read, execute, write into various data forms of zeek logs. 
<br><br>
A fitting example would be to address our earlier problem. This package has devloped functions that can efficiently read zeek logs into dataframes or matrices in one go. Hence no more fumbling around extracting header data and log data from the zeek logs. A simple tool that cuts down on tons of effort. Apart from that they have developed many more tools to perform integration with various other tools and packages such as : scikit learn , matplotlib, kafka etc. They also have provided a vast variety of examples to demonstrate the usage of zeek and with baby steps into usage of the log data with the above mentioned packages and data.
<br>br>
link : https://github.com/SuperCowPowers/zat

### Finally 
Theres a large and diverse open source community contributing to scripts and various other new things. While it may be relatively new it has lot more people joining and contributing every single day. To see this growth and find out more please visit the zeeks github with around 50 repositories, you can visit and explore any sub topic to your liking.
<br> Zeek : https://github.com/zeek
