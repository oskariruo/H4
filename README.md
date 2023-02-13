<h1>H4 Hash</h4>

<h2>One-way functions</h2>

- One-way functions, like the name suggest, act better in the pre set direction. As in, they are relatively easy to compute but harder to reverse. "Harder" meaning that technically it could be done, but it would take millions of years and countless resources etc.

- A real world example could be breaking a plate into hundreds of pieces. Fixing the plate back to it's original form would take a lot more time than what took to break it.

- A trapdoor one-way function resembles a one-way function, but it additionally has a secret trapdoor. This allows for the function to be hard to reverse on it's own but easy to reverse if the trapdoor is known

<h2> One-Way Hash Functions </h2>

- One-way hash functions converts a variable to a fixed-length output string, called the hash value. If one character is changed in the input string, the hash value will be complitely different. This is the main reason one-way hash functions are used to create hashes.

- Usually, one-way hash functions are used to verify the integrity of a message. This means that the hash value can be used to verify that the message has not been changed.

<h2>Source</h2>

https://learning.oreilly.com/library/view/applied-cryptography-protocols/9781119096726/10_chap02.html#chap02-sec003


<h2>Hashcat</h2>

<h3>Hashcat Installation</h3>

First step is to install Hashcat. Hashcat is a password recovery tool with support for a variety of hash types.

For Linux install use the commands to first do updates/upgrades before (a good practice overall) and then install the hashcat:

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get -y install hashid hashcat wget
```

<h3>Directory for Hashcat</h3>

A good practice is to create a new dedicated directory for the hashcat files:

```
$ mkdir hashing
$ cd hashing
```

<h3>Download directory file</h3>

Download the rockyou.txt file as it will be used to crack the hashes:

```
$ wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
$ tar xf rockyou.txt.tar.gz
$ rm rockyou.txt.tar.gz
```

<h3>Indentifying hash type</h3>

Cracking hash 8eb8e307a6d649bc7fb51443a06a216f requiers for us to know the hash type. This can be done by using the hashid tool:

```
$ hashid -m 8eb8e307a6d649bc7fb51443a06a216f
```

output looks like this:

```
Analyzing '8eb8e307a6d649bc7fb51443a06a216f'
[+] MD2 
[+] MD5 [Hashcat Mode: 0]
[+] MD4 [Hashcat Mode: 900]
[+] Double MD5 [Hashcat Mode: 2600]
[+] LM [Hashcat Mode: 3000]
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 [Hashcat Mode: 8600]
[+] Skype [Hashcat Mode: 23]
[+] Snefru-128 
[+] NTLM [Hashcat Mode: 1000]
[+] Domain Cached Credentials [Hashcat Mode: 1100]
[+] Domain Cached Credentials 2 [Hashcat Mode: 2100]
[+] DNSSEC(NSEC3) [Hashcat Mode: 8300]
[+] RAdmin v2.x [Hashcat Mode: 9900]
```
We can see from the output that we have the kind of hash that is possible for our hash. Generally one of the 3 to come up first should work. I tried MD5, because it's popular. So, MD5 hash and hashcat mode for it is 0.

<h3>Final crack</h3>

Now with the knowledge we have on the hash we can run the following command:

```
$ hashcat -m 0 '8eb8e307a6d649bc7fb51443a06a216f' rockyou.txt -o solved
```

then the following cat command to show the password:

```
$ cat solved
```

and the output is:

```
8eb8e307a6d649bc7fb51443a06a216f:february
```

Answer is february!

<h2>Source</h2>

https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

<h2>John the Ripper Compilation</h2>

<h3>Installing prerequisites</h3>

First step is to install the prerequisites for John the Ripper. Use following commands to update and upgrade (good practice) and then install the prerequisites:

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget
```

<h3>Downloading and compiling John the Ripper</h3>

The second step is to download and compile John the Ripper by using the commands:

```
$ git clone --depth=1 https://github.com/openwall/john.git
$ cd john/src/	
$ ./configure
$ make -s clean && make -sj4
```

and then using this command to run it:

```
$ $HOME/john/run/john
```

<h2>Cracking with John the Ripper</h2>

Download the file that needs to be cracked. For this we are using the "tero.zip"-file:

```
$ wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip
```

to unzip this file we need to password...

<h2>Zip file cracking</h2>

First step is to extract the hash form the file into file called for example in this case tero.zip.hash:

```
$ $HOME/john/run/zip2john tero.zip >tero.zip.hash
```

and then to view the hash:

```
$ cat tero.zip.hash
```

and the output looked like this:

```
tero.zip/secretFiles/SECRET.md:$pkzip$1*1*2*0*b7*d9*4c752c85*46*4f*8*b7*572b*6fc2fc774ed6b264ebea4c64b1b1ae935507abd1ca544987e878fcad58bb132bc60240152c250dbfcc07b47b348b7ac4f2ae938cceeca978e258b0f1bd2fc7096ad24760a9e20353c75c6588413da66e98dc620e6d9e7f3abc73fd5a12ce1c205072efa1f55bf8d6a06ed7e7998ad0a921d4a3dd8b7bfb3fbc96c2fce5640a87554bb002ab5e6153ca10850ee79bdfa5c85ce4e6b446f972735c5385f3239e182e2c4e59214eb03a6aee636631fec207d9d3eb7560c83d*$/pkzip$:secretFiles/SECRET.md:tero.zip::tero.zip
```

and the step after that is to perform actual hacking against the hash:

```
$ $HOME/john/run/john tero.zip.hash
```

and the output looked like:

```
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
0g 0:00:00:00 DONE 1/3 (2023-02-11 17:51) 0g/s 1690Kp/s 1690Kc/s 1690KC/s Mdzip1900..Msecret1900
Proceeding with wordlist:/home/oskru/john/run/password.lst
Enabling duplicate candidate password suppressor
butterfly        (tero.zip/secretFiles/SECRET.md)     
1g 0:00:00:01 DONE 2/3 (2023-02-11 17:51) 0.8130g/s 72034p/s 72034c/s 72034C/s 123456..bob123
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

the password is highlighted and it is "buttefly".

<h3>Zip file extraction</h3>

Now that the password is know the previous file can be extracted:

```
$ unzip tero.zip
```

and the output looks like: 

```
Archive:  tero.zip
[tero.zip] secretFiles/SECRET.md password: 
  inflating: secretFiles/SECRET.md
```

and cat to look at the file:

```
$ cat secretFiles/SECRET.md
```

output and final answer:

![image](https://user-images.githubusercontent.com/78789083/218529829-0f91f219-858e-4d0e-b60e-fe520ee9420e.png)

<h2>Source</h2>

...



