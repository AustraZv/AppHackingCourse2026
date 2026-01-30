## a) 
I downloaded the ezbin-challanges(https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip) and compiled passstr.
I had no familiarity with strings before and decided to check the man pages. 
After that I ran the program and found the flag 
<img width="522" height="87" alt="Screenshot 2026-01-28 at 16 15 42" src="https://github.com/user-attachments/assets/bb538030-639c-4b29-9f19-83343126b675" />

## b)
My idea for this was to use hashing, specifically sha256.
I read up on hashing in C, there were some libraries online that were really simple but I wanted to use something widely supported like openssl. Looking through openSSL documentation, I found that the SHA256 function was no longer supported, and instead you had to use EVP(https://docs.openssl.org/1.0.2/man3/sha/#description). Even though I could have used this function, I wanted to learn the most up to date method. I did not want to pipe to sha256sum either do to possible leakage issues. I had difficulties finding good documentation on the usage of EVP but the openSUSE man pages had a clear description of the functions(https://manpages.opensuse.org/Leap-16.0/openssl-3-doc/EVP_DigestUpdate.33ssl.en.html#EVP_DigestUpdate()).  
This github post showed me how the function was implemented (https://github.com/openssl/openssl/issues/19612) and this discussion showed me that I should format the key as hex numbers and that I should use memcmp instead of strcmp (https://stackoverflow.com/questions/37193767/compare-hashes-in-c)

After this, I looked up on how to run sha256sum on a string, and found this suggestion (https://stackoverflow.com/questions/3358420/generating-a-sha-256-hash-from-the-linux-command-line)

After running sha256sum on the password(sala-hakkeri-321) i got this hash 
<img width="1006" height="74" alt="image" src="https://github.com/user-attachments/assets/03216779-7d61-4443-88fd-b5082992a65f" />


Initially the file failed to compile, which is when i discovered that I needed a symlink and I implemented that. (https://stackoverflow.com/questions/40392021/openssl-evp-h-file-not-found-os-x-mongodb) 
