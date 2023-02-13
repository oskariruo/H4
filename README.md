<h1>H4 Hash</h4>

<h2>One-way functions</h2>

- One-way functions, like the name suggest, act better in the pre set direction. As in, they are relatively easy to compute but harder to reverse. "Harder" meaning that technically it could be done, but it would take millions of years and countless resources etc.

- A real world example could be breaking a plate into hundreds of pieces. Fixing the plate back to it's original form would take a lot more time than what took to break it.

- A trapdoor one-way function resembles a one-way function, but it additionally has a secret trapdoor. This allows for the function to be hard to reverse on it's own but easy to reverse if the trapdoor is known

<h2> One-Way Hash Functions </h2>

- One-way hash functions converts a variable to a fixed-length output string, called the hash value. If one character is changed in the input string, the hash value will be complitely different. This is the main reason one-way hash functions are used to create hashes.

- Usually, one-way hash functions are used to verify the integrity of a message. This means that the hash value can be used to verify that the message has not been changed.


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

<h2>John the Ripper Compilation</h2>




