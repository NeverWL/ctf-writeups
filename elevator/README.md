# Retrieving Flags from the Elevator challenge

This is my solution for retrieving the flag from the second CTF league meeting.

## Level 0

After tunneling into the OSUSec server using ssh chal@elevator.ctf-league.osusec.org -p 1302, I used ls to see the files, cat to read README, then cd /creds_level1 to enter the directory, cat /creds_level1.txt to retrieve the user and password for the next level of credentials, and then su --login level1_30117 to proceed to the next level. The authentication process is the same for all following levels so I won't repeat its explanation.

## Level 1

In order to quickly scan through the directory, I followed the suggestion of my teammate Patrick and performed grep -rni to recursively scan through the files in the directory to retrieve the password. The two commands I used were:

```bash
grep -rni "Password: " 2zoevnwn
grep -rni "level: " 2zoevnwn
```

This gave me the credentials to proceed.

## Level 2

This time I used grep on creds_level3.txt for "password" andd "level" to retrieve the credentials.

## Level 3

I used sh to run creds_level4.sh and retrieve the credentials.

```bash
sh creds_level4.sh
```

## Level 4

I used ls -a to find the .hidden_creds_level5 directory, which I proceeded to grep -rni to retrieve the credentials from.

## Level 5

I just used cat to read the file and proceeded.

## Level 6

I used cat and retrieved cat to retrieve the credentials from creds_level7.txt.

## Level 7

With the appropriate credits, I ran cat/flag.txt and retrieved the flag for this challenge.