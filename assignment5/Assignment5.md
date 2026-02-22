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



