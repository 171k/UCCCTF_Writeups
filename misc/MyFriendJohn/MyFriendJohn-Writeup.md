# My Friend John - Writeup

This challenge is nice because it is teaching us on how to use `John The Ripper`!

> #### Description
> 
> Help me recover the password!!!
> 
> [Zip Download](https://ucc-ctf.noorraihan.com/files/0300af01401d07168e6cb582828e9717/My_Friend_John.zip?token=eyJ1c2VyX2lkIjo4NywidGVhbV9pZCI6bnVsbCwiZmlsZV9pZCI6MjV9.aSVHjw.2m1sQazuJXDFh9eDsX3EcJ3LKJk)

We also got a zip file that locked with a password. Since the challenge name is `my friend john` we can assume that we are going to use the tool `john` to solve this challenge. 



So the first step is we need to convert this zip into hash. We can simply perform this in many ways but one of it is using the tool `zip2john`:

```bash
zip2john My_Friend_John.zip > john.hash
#this is simply just to convert the zip into hash!
```

Then, since we already got the file hash, we can try bruteforce the password with dictionary attack via rockyou.txt (one of the famous wordlist). The command shall be:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt john.hash

#if you dont have rockyou.txt inside your wordlists, you can simple download the rockyou.txt online and import it
```

Then the output should look like this:

> Using default input encoding: UTF-8
> Loaded 1 password hash (PKZIP [32/64])
> No password hashes left to crack (see FAQ)

This indicates that our attack is successful since it mentioned `loaded 1 password hash`. To read the password loaded, simply run:

```bash
john --show john.hash
```

![Screenshot 2025-11-25 145133.png](Screenshot%202025-11-25%20145133.png)

1 password hash cracked which is the `9112iloveyou`. Use this password to extract the zip and get the flag!



Or so i thought but we are not done yet.. there is another zip with password and a `Readme.txt`.

> #### Readme.txt
> 
> WELL DONE!!!
> 
> i forgot the pass for flag.zip, can u help me recover the pass. i dont remember the pass but i know the pass contain name of organisation, one symbol and four number. can u help me recover the password
> 
> sincerely:
> 
> uccuitm



okay now this is getting more interesting. We already know the pattern of the next password so we are going to perform mask attack on the zip file based on the Readme.txt provided. Lets sort things out first:



> #### The password pattern based on Readme.txt
> 
> - contains name of organization, which I assume "uccuitm"
> 
> - has one symbol
> 
> - have 4 numbers

Now from here on, there 2 ways to attack, directly using `john` mask attack or use `crunch` to create custom wordlist before attacking.



## Using Crunch

crunch is a tool to generate a custom wordlist, we simply have to feed it with patterns and it will generate the wordlist for us.



How to use crunch:

```bash
crunch min_length max_length -t insert_pattern -o wordlist_name.txt
# -t stands for template mode
#we are going to use % to represent digit and @ to represent symbol
# -o stands for output
```

So the command shall be:

```bash
crunch 12 12 -t uccuitm@%%%% -o wordlist.txt
```

The output:

![Screenshot 2025-11-25 150549.png](Screenshot%202025-11-25%20150549.png)

This shows that crunch successfully created a wordlist with 3MB lines. Thats alot!



Now, using the same command earlier, we will crack the password of the zip (dont forget to turn into hash first!) :

```bash
john --wordlist=customattack.txt flag.hash
```

![Screenshot 2025-11-25 150932.png](Screenshot%202025-11-25%20150932.png)

and.. there it is, we got the password! now we can unzip and get the flag!



flag:

`UCC{#######_####_####_#########}`

unfortunately, I will not reveal the flag here. You need to try it out yourself hehe.


