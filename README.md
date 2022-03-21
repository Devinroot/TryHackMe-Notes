## TryHackMe Notes

### Complete Beginner:

#### Research on the Internet 

==learn to Google usefully==

In the Burp Suite Program that ships with Kali Linux, what mode would you use to manually send a request (often repeating a captured request numerous times)? -> Repeater



What hash format are modern Windows login passwords stored in? -> NTLM

##### Vulnerability Searching:

- [ExploitDB](https://www.exploit-db.com/)     command `searchsploit fuel cms` in Linux

  Know the CVE number `CVE-YEAR-NUMBER`

- [NVD](https://nvd.nist.gov/vuln/search)

- [CVE Mitre](https://cve.mitre.org/)

##### Manual Pages: Man Command

use man to know the syntax and rules 

`nc -l -p 12345`



#### Linux Fundamentals

Research: What year was the first release of a Linux operating system? -> 1991

`find -name passwords.txt`         `find -name *.txt`   search the file's path

`grep "81.143.211.90" access.log `   在access.log文件夹下寻找是否有此ip内容



##### Shell Operators :

| Symbol / Operator | Description                                                  |
| ----------------- | :----------------------------------------------------------- |
| &                 | This operator allows you to run commands in the background of your terminal. 在后台执行这个指令 |
| &&                | This operator allows you to combine multiple commands together in one line of your terminal.     ==command1&&command2== 只有在command2成功时才会运行command1 |
| >                 | This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere. |
| >>                | This operator does the same function of the `>` operator but appends the output rather than replacing (meaning nothing is overwritten). |

```shell
& 在后台执行
command1 && command2
echo hey > welcome
创建一个名为welcome的文件，内容包含“hey”
> 覆盖内容
>> 不覆盖内容
```

`ssh name@MACHINE_IP` 

What flag would we use to display the output in a "human-readable" way?  -> ls -h



| Command | Full Name      | Purpose                      |
| ------- | -------------- | ---------------------------- |
| touch   | touch          | Create file                  |
| mkdir   | make directory | Create a folder              |
| cp      | copy           | Copy a file or folder        |
| mv      | move           | Move a file or folder        |
| rm      | remove         | Remove a file or folder      |
| file    | file           | Determine the type of a file |

==Tips==:    `rm -r`only files, not for folder /current file

 			`rm -R` not only files but also folder / 递归删除

- su user2        our new session drops us into our previous user's home directory. 
- su -l user2     our new session has dropped us into the home directory of "user" automatically. 



##### ==Common Directories==

/etc   how your system stores the passwords for each user in encrypted formatting called sha512.

/var  ==log files==

/root 

/tmp Similar to the memory on your computer, once the computer is restarted, the contents of this folder are cleared out.

> What root directory is similar to how RAM on a computer works? -> /tmp

##### Terminal Text Editors: nano vim

##### Downloading Files

###### wget

`wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt`

###### SCP

- Copy files & directories from your current system to a remote system ==从当前系统拷贝文件到远程机器==

- | Variable                                                    | Value           |
  | ----------------------------------------------------------- | --------------- |
  | The IP address of the remote system                         | 192.168.1.30    |
  | User on the remote system                                   | ubuntu          |
  | Name of the file on the local system                        | important.txt   |
  | Name that we wish to store the file as on the remote system | transferred.txt |

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

- Copy files & directories from a remote system to your current system 从远程机器拷贝文件到当前机器

| Variable                                                    | Value           |
| ----------------------------------------------------------- | --------------- |
| The IP address of the remote system                         | 192.168.1.30    |
| User on the remote system                                   | ubuntu          |
| Name of the file on the local system                        | important.txt   |
| Name that we wish to store the file as on the remote system | transferred.txt |

` scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

###### Web

> curl      wget

`python3 -m http.server`

`wget http://127.0.0.1:8000/file`

##### Processes 101

view Processes `ps`     `ps aux` all  users

`top`  gives you real-time statistics about the processes running on your system instead of a one-time view. These statistics will refresh every 10 seconds, but will also refresh when you use the arrow keys to browse the various rows.

`kill PID`  i.e. `kill 1337`



Below are some of the signals that we can send to a process when it is killed:

- ==SIGTERM== - Kill the process, but allow it to do some cleanup tasks beforehand  允许事先执行一些清理任务
- ==SIGKILL== - Kill the process - doesn't do any cleanup after the fact
- ==SIGSTOP== - Stop/suspend a process

###### Processes Start

```shell
PID1 systemd
```

Start on Boot:

`systemctl [option] [service]`

options:

- start
- stop
- enable
- disable

i.e. to tell the apache2 to startup -> `systemctl start apache2`

i.e. What command would we use to start the same service on the boot-up of the system?

`systemctl enable myservice`

###### Use oprator& to run in the background

`echo "hello world" &`

Ctrl + z to pause

`fg`

What command would we use to bring a previously backgrounded process back to the foreground? -> fg

###### Automation

`crontabs`

A crontab is simply a special file with formatting that is recognised by the `cron` process to execute each line step-by-step. Crontabs require 6 specific values:

| Value | Description                               |
| ----- | ----------------------------------------- |
| MIN   | What minute to execute at                 |
| HOUR  | What hour to execute at                   |
| DOM   | What day of the month to execute at       |
| MON   | What month of the year to execute at      |
| DOW   | What day of the week to execute at        |
| CMD   | The actual command that will be executed. |

i.e. Let's use the example of backing up files. You may wish to backup "cmnatic"'s "Documents" every 12 hours. We would use the following formatting: 

`0 *12 * * * cp -R /home/cmnatic/Documents /var/backups/`

Crontabs can be edited by using `crontab -e`

###### log

/var/log/



#### Network Exploitation Basics

APSTNDP

Application Presentation Session Transport Network Data link Physical

Layer 7 -- Application:

Providing an interface to transmit data.

> FTP Protocol communicate with!

Layer 6 -- Presentation:

handling any encryption, compression or other transformations to the data. 

Layer 5 -- Session:

Set a connection / simultaneously

Layer 4 -- Transport:

choose the protocol

> TCP(Transmission Control Protocol) 传输控制协议
>
> A TCP connection allows the two computers to remain in constant communication to ensure that the data is sent at an acceptable speed, and that any lost data is re-sent.   ==for situations where accuracy is favoured over speed==(e.g. file transfer,or loading a webpage)
>
> 
>
> UDP(User Datagram Protocol) 用户数据包协议
>
> Packets of data are essentially thrown at the receiving computer  ==be used in situations where speed is more important==(e.g. video streaming)

With a protocol selected, the transport layer then divides the transmission up into bite-sized pieces (over TCP these are called ==*segments*==, over UDP they're called ==*datagrams*==), which makes it easier to transmit the message successfully. 

Layer 3 -- Network:

Locating the destination of your request 

==Logical addressing== -> IP address





Layer 2 -- Data Link:

Receive a packet from the network layer and check they haven't been corrupted

add in the physical(MAC) address of the receiving endpoint

接收来自网络层的数据包，并添加接收端点的物理（MAC）地址

> Every network enable computer has a NIC(Network Interface Card) which comes with a unique MAC(Media Access Control) to identify it.
>
> ==Actually, MAC address can not be changed--although can be spoofed欺骗, it's just a physical address==

Data is formatted in preparation for transmission!

Layer 1 -- Physical:

The hardware of the computer

