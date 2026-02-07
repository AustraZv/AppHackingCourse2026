## a) Installing Ghidra
I already had Ghidra installed on my computer
## b) Find password in packd and analyze the program 
https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip
First I unpacked the library with 'upx -d packd' 
Then I Imported it to Ghidra, and renamed variables to be more readable
<img width="1288" height="660" alt="image" src="https://github.com/user-attachments/assets/7cce8c2b-c63b-4bb4-b1ce-78f02621d2b3" />

From this code, we can infer that the program takes an incoming string from the terminal, compares it to the string "piilos-AnAnAs" and assigns the comparision an int value.
If the string's match then the int value is assigned to 0, and it outputs a confirmation that it is the password, with the flag.

If the strings do not match, it outputs "Sorry, no bonus."

After this the program exits with a return value of 0.

## c) Modifying the passtr program without the original source code to output at every false password, but not the correct one
https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip

After downloading the program I ran ghidra. 
<img width="632" height="380" alt="Screenshot 2026-02-06 at 19 28 22" src="https://github.com/user-attachments/assets/58bc8776-e053-4601-9d3f-b4ee5fa92c5a" />

From here we can see that the password is "sala-hakkeri-321".
The program works much like the previous one, compares the strings and outputs if they match.
By changing the if statement to '(check!=0)' we can achieve the opposite result.

I exported the code to a c file and applied the change, as well as changing the names of some functions and adding the neccesary libraries to make the code compile.


<img width="610" height="558" alt="Screenshot 2026-02-07 at 18 17 10" src="https://github.com/user-attachments/assets/1c952686-cc07-4f0b-b525-257b7bab495e" />

And as we can see, the output is inverted. 
<img width="550" height="87" alt="Screenshot 2026-02-07 at 18 19 28" src="https://github.com/user-attachments/assets/2bbd1c2b-9e80-4f7f-aeb1-552049ed8bcc" />


## d) Nora crackme
https://github.com/NoraCodes/crackmes

I cloned the git repository, then I had some issues compiling with the Make which I attribute to the fact that I am using a mac. After that I ran gcc by itself and I specified an output file. After that the binaries were ready to be inspected and tested.

## e) Nora crackme01
This was quite simple, I analyzed the file in Ghidra, renamed variables and immideatly noticed that the password is password1

<img width="1322" height="1138" alt="image" src="https://github.com/user-attachments/assets/ebf8f28b-d446-47c9-bf4c-2bcd8be47462" />

And it worked.
<img width="610" height="73" alt="Screenshot 2026-02-07 at 16 48 01" src="https://github.com/user-attachments/assets/17e9a116-521f-434a-b146-f9a55956ee91" />

## Nora crackme01e
Once again, I analyzed the file in Ghidra, and noticed the password immideatly.

<img width="1616" height="1110" alt="image" src="https://github.com/user-attachments/assets/674d910d-e82d-4815-9406-676c7e4a79d0" />

The difference here is that the formatting uses non-standard characters, so the command has to be one of these variations

<img width="1220" height="146" alt="image" src="https://github.com/user-attachments/assets/793fc709-0198-413c-aa80-bd2ba76b5ef1" />

## f) Nora crackme02
I analyzed the file in ghidra, renamed the variables, retyped the input variable to char*,  created an automatic structure and changed the field type to char*.
<img width="1158" height="1226" alt="image" src="https://github.com/user-attachments/assets/20d49c04-801b-41ef-acea-a3d92016a95e" />

First the program checks if the program has 2 arguements exactly(and the program name), the program proceeds, otherwise it tells the user that it only needs one arguement.

Then it initializes an increment variable, that will increment by 1 each cycle.

Then it launches the while loop, which will proceed until the 'break' arguement.
It initializes a checking variable, giving it the value false.

First it checks if the current character of the password string is null, if it is not, it sets the value of the checking variable to true if the current character is not a null, and to false if it is. 
This can check for the end of the string, but also can allow a null output to be flagged as true.

Then, the program checks the value of the checking variable, if it is false, it returns 0, otherwise it proceeds. 

In the final part of the loop the program checks weather the character[i]  of the input string matches the corresponding character of the password string with 1 subtracted from its ASCII value. Otherwise, it exits the loop.  This effectively checks if the input string matches the password string that is encoded with ROT -1 

After the loop, if the loop managed to break without returning, the program outputs that the password is false and sets the return value to 1.

From this we can infer 2 solutions
1) pass an empty string
2) encode the password string to ROT -1

   First solution
   <img width="944" height="66" alt="image" src="https://github.com/user-attachments/assets/d5b70d5e-d41b-40b1-9386-31c7e47232a3" />

   As for the second, I used to do cryptography puzzles in my free time, and as a result I know a site that has a large library of tools for encoding with old-school ciphers - dcode.fr

   I encoded the string with these parameters
   <img width="864" height="764" alt="Screenshot 2026-02-07 at 17-09-08 ROT Cipher - Rotation - Online Rot Decoder Solver Translator" src="https://github.com/user-attachments/assets/becfb8fc-5029-48ea-a5ec-56c6575eb4c0" />

and got this output 

<img width="652" height="96" alt="image" src="https://github.com/user-attachments/assets/226529ac-036e-4e8d-9e69-490583073ff8" />

After testing it, we can see that it works.

<img width="968" height="68" alt="image" src="https://github.com/user-attachments/assets/3e3a17be-8f25-4f3e-8767-44ab4541f00c" />


## g) Additional solutions in crackme01
As the program only compares the length of the password string, any characters after it are ignored, therefore we can pass any output as long as our string starts with "password1"
<img width="1118" height="150" alt="image" src="https://github.com/user-attachments/assets/de953266-7f08-4611-a038-009c2cdb4449" />

## h) Additional solutions in crackme02
Already covered in f)

## i) Solve crackme02e

The program is very simmilar and we can see the same two solutions, except this time the program subtracts -2 from the password character's ASCII values, which means it expects an output encoded with ROT -2

The first solution is the same - empty string

<img width="866" height="74" alt="image" src="https://github.com/user-attachments/assets/0354c1e1-fe0b-482d-a89b-1b3a396fafa0" />

And 'yuvmnpoi' encoded with ROT -2 is 'wstklnmg'
<img width="1056" height="66" alt="image" src="https://github.com/user-attachments/assets/ffbec1f8-b4b3-4d54-87f7-f1cf1455c54f" />

As we can see, both solutions work.
