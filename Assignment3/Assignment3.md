## a) finding the flag in ezbin challanges passtr
I downloaded the ezbin-challanges(https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip) and compiled passstr.
I had no familiarity with strings before and decided to check the man pages. 
After that I ran the program and found the flag 
<img width="522" height="87" alt="Screenshot 2026-01-28 at 16 15 42" src="https://github.com/user-attachments/assets/bb538030-639c-4b29-9f19-83343126b675" />

## b)
My idea for this was to use hashing, specifically sha256.
I read up on hashing in C, there were some libraries online that were really simple but I wanted to use something widely supported like openssl. Looking through openSSL documentation, I found that the SHA256 function was no longer supported, and instead you had to use EVP(https://docs.openssl.org/1.0.2/man3/sha/#description). However, both on my macbook and my linux desktop I could not get the openssl library to be properly recognized, online.  I got the library working on my Macbook but the library was x86 only, and I could not get it working quickly on opensuse so I gave up. I ended up using this library (https://lucidar.me/en/dev-c-cpp/sha-256-in-c-cpp/), and I followed the instructions given.
After this, I looked up on how to run sha256sum on a string, and found this suggestion (https://stackoverflow.com/questions/3358420/generating-a-sha-256-hash-from-the-linux-command-line)

After running sha256sum on the password(sala-hakkeri-321) i got this hash 
<img width="1006" height="74" alt="image" src="https://github.com/user-attachments/assets/03216779-7d61-4443-88fd-b5082992a65f" />

this was the original code 
<img width="863" height="221" alt="image" src="https://github.com/user-attachments/assets/05dbed72-09e0-424d-b9d2-16af4197a52c" />

and this was the edited code 
<img width="847" height="353" alt="image" src="https://github.com/user-attachments/assets/5682d8c6-d628-4a70-83f4-6e74317ea149" />
(ignore  the errors, that is from the library being in a  serperate folder)
I compiled it with this command
```
g++ -o passtr "passtr.c" "lib/sha256.c"  -I "lib"
```
The program now operates correctly, and does not show the password in strings.
<img width="715" height="464" alt="image" src="https://github.com/user-attachments/assets/5ebc5d76-f056-4cc4-bfd8-72bd12c1ef2a" />




## c) packd in ez-bin challanges
I first viewed it in strings and observed 2 things, 
1) piilos-An looked to be the password
2) Sorry was turned to S1rry
<img width="796" height="634" alt="image" src="https://github.com/user-attachments/assets/fe8d5d8e-5b84-4db8-a9f0-2c77640dcd4d" />


My first guess was that this was a simple XOR, or another simple cypher. As I have decoded these  as a hobby in the past, I went to my favorite website for decoding and analysing simple cyphers(dcode.fr).

The xor was not promising, nothing that looked like a possible password.
I then tried some other cyphers, and on the Viginiere cypher page, there was a section on how to identify a cypher, the page mentioned that the name of the file/book/excersise you found could give you a clue(in this case it wasindicating references to france  for Viginiere). This is where I had 3 realizations.
1) the name of the file was packd.
2) In my 12th grade programming class we studied basic compression algorithms, one of them involved replacing common character sequences with numbers(for example to indicate the position of the original characters), suddenly S1ry makes a  lot more sense  if 1 is substituting or.
3) the strings output mentions that it was packaged with UPX, which is a compression software

After this, I downloaded UPX and checked the man pages to see how to decompress it, just a -d flag worked and I got the correct flag
<img width="748" height="532" alt="image" src="https://github.com/user-attachments/assets/69bcbaff-3468-46ba-9353-c378b20a46a2" />


