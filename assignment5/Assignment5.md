## a) lab1.zip https://terokarvinen.com/application-hacking/lab1.zip
Attempting to run the program with gdb caused a Segmentation fault at line 7
<img width="582" height="80" alt="image" src="https://github.com/user-attachments/assets/a1e0c5a1-3306-48d4-b1bc-4c2ddc6aa02d" />


<img width="551" height="345" alt="image" src="https://github.com/user-attachments/assets/11b57926-fd6e-4d7f-92d1-848cd166d488" />


Looking at the entire code it is obvious why, the loop does not check if the message is null, or if there are characters left thus the program tries to access memory  that does not exist and fails.
<img width="739" height="453" alt="image" src="https://github.com/user-attachments/assets/21b2c947-c4f0-4b5c-8816-c49968dcddb8" />

replacing the do while loop with a while loop solves these issues + with the introduction of length checking and null checking the program can now avoid segfaults.



## b) lab2.zip https://terokarvinen.com/application-hacking/lab2.zip
There was no debug information in the program, so I solved  this task with a combination of strings and ghidra.

First I loaded the program into ghidra, did some variable renaming, and automatic creation of structures.
Some of the code I did not comprehend fully, such as the for loop at the start, my  guess it is related to the flag.

<img width="489" height="627" alt="image" src="https://github.com/user-attachments/assets/b784b191-e61a-4525-907d-b4ed6837a3b1" />

The code copies out the negative output at the start, and stores it in the same pointer as the inputted password.
It recieves the input, calls the comparision function, and then calls the output function if the password is correct, if it isnt it just outputs "sorry No bonus".

This is the comparision function, it checks if i is odd or even, and rotates characterby 3 or-7 respectively.
<img width="474" height="515" alt="image" src="https://github.com/user-attachments/assets/91ab83f3-452e-4460-95b5-ecd7e7c5ee2f" />


Finding the password proved a bit more challanging, however, 2 things helped me discover it, first I ran strings, which gave me this result.
Strings gave this output.
<img width="354" height="144" alt="image" src="https://github.com/user-attachments/assets/087be8b3-e160-4372-a159-74680a7e7f97" />

One of the strings was identical to the value being passed to the comparision function converted to char but in reverse order

<img width="354" height="144" alt="image" src="https://github.com/user-attachments/assets/3e96468b-139f-4a73-a7aa-43664bb5cd40" />

When I misconfigured the inputOutput variable to int, ghidra showed the "Sorry n" string as reversed, and thus I decided to attempt to encode "anLTj4u8".

I copied and slightly modified the code from the comparision function 
<img width="545" height="332" alt="image" src="https://github.com/user-attachments/assets/615abb45-7e41-456a-bbcc-e2457e92c810" />

It produced this string - "dgOMm-x1"
<img width="483" height="107" alt="image" src="https://github.com/user-attachments/assets/4a66cde6-55ae-4737-b10c-9ac8ef8b88f1" />

And I tested it, and I got the flag.

## c) crackme03 and 04

# crackme  03
After analyzing the code in ghidra I realized that the program takes 2 strings,  "lAmBdA" and "\x02\x03\x02\x03\x05"(corresponding to ascii characters number 2,3,2,3,5), and sums them, effectively rotating the string  with a unique character.
<img width="566" height="590" alt="image" src="https://github.com/user-attachments/assets/dafae674-d0fa-4842-80b0-cf7f61490e0e" />

<img width="483" height="107" alt="image" src="https://github.com/user-attachments/assets/d8e55c8d-dbd3-408a-817a-ac617eddc7e2" />

As a result we get nDoEiA  
<img width="543" height="39" alt="image" src="https://github.com/user-attachments/assets/9722ddf0-4a71-44cb-a7cf-2233753d30ec" />

# crackme04

After analyzing the code  in ghidra, I realized that the code expected a 16 character string where all the ascii values sum to 1762
<img width="657" height="919" alt="image" src="https://github.com/user-attachments/assets/abfde4c6-6ac0-4348-9b9b-0923385e7d1e" />

1762/16=110 mod 2
so if we output 1 character  with the value 112, and 15 with the value 110, we should get in. 
112 = p, 110 = n
<img width="614" height="39" alt="image" src="https://github.com/user-attachments/assets/7ebc84db-936a-4fdd-931e-f055170ee1f1" />






