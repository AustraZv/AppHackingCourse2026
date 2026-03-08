## x)
Schneier 2015: Applied Cryptography, 20ed: Chapter 1: Foundations:
I've read this before, and I am already familiar with the terminology and as I have had education in boolean algebra, I am quite familiar with how xor works on bits.

The large numbers segment is a good illustration of probability

Karvinen Python basics for hackers:

I've done python before, but haven't done it in this context that much, its a good and comprehensive guide.
# Cryptopals set 1
Tasks done in python
## cryptopals 1
The first task was relatively simple, I encoded the string as a bytearray and encoded it in base 64
<img width="746" height="338" alt="image" src="https://github.com/user-attachments/assets/01c06911-95f0-4c62-b59d-2091e8a75b05" />
<img width="974" height="32" alt="image" src="https://github.com/user-attachments/assets/414fa3fb-87d9-4972-b045-d393d1cac2b6" />

## 2
The second task was simple as well, I just formatted both strings as bytearrays and xor'd each byte against each other, I had to format the result as hex because otherwise it printed it as text
<img width="1124" height="434" alt="image" src="https://github.com/user-attachments/assets/54885a55-fc38-45cb-9b19-03e46bf023e8" />
<img width="588" height="34" alt="image" src="https://github.com/user-attachments/assets/ce9de68b-870d-47af-86a8-588ffbc61191" />

## 3
In the third task I took advantage of the property of xor that if a^b=c then a^c=b. therefore, instead of brute forcing the key, I xor'd the most frequent letters of the english alphabet against the most frequent characters of the cyphertext(modifying which element manually), and then testing the key to get the result
<img width="1482" height="636" alt="image" src="https://github.com/user-attachments/assets/03323002-5d3c-4efa-b35c-71891dfd9379" />

<img width="776" height="36" alt="image" src="https://github.com/user-attachments/assets/7a57d62b-b928-4a64-9fbe-13ee13f0dca2" />

## 4
Initially I tried a simmilar approach, but ran into difficulties, I had to lookup a hint and implemented dictionary recognition as seen here https://medium.com/@tarunrd77/cryptopals-cryptography-solutions-set-1-cf4df0132614 

However, my code achieves the result in a lot less attempts, because instead of bruteforcing each byte, I am only testing the most frequent characters of english and of the cyphertext against each other
<img width="670" height="82" alt="image" src="https://github.com/user-attachments/assets/6d54e7c3-bb8a-4b22-935f-485d0cc914a5" />
Complete code here: https://github.com/AustraZv/cryptopals/blob/main/cryptopals4.py
## 5
I just took the string and the key, and xor'd each byte against each other, and achieved the result
<img width="1890" height="634" alt="image" src="https://github.com/user-attachments/assets/e3b59742-28ab-4242-9ad4-2f9cbc2595a3" />
<img width="2140" height="72" alt="image" src="https://github.com/user-attachments/assets/e9be16c7-bb12-40aa-9591-fe5400694856" />
