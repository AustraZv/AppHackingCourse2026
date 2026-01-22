## x)
# OWASP A01:2021
These are things I mostly knew already, the need for access management is very easy to comprehend, yet thousands of websites are still online with no protection. I remember watching a security talk where a lot of websites that should not have been public were shown, one of them was a control panel to a  French hydroelectric power plant. All online, no access control, nothing.

The methods listed here for prevention are not that difficult to implement, and to me, its quite mindboggling that some organizations are still willing to sacrifice their security for minor convinience in administration.


# Find Hidden Web Directories - Fuzz URLs with ffuf - Tero Karvinen

I am writing this after doing the practical exercises, and ffuf was very easy to understand and the ammount of documentation it comes with is amazing. 

# Access control vulnerabilities and privilege escalation - Portswigger
It still is suprising to me that some people still submit settings in the URL or base HTML, it feels like some developers do not know about inspect element or the fact that URL's can be changed. 


## a)

First I set up the system, I was out of home when I did this and did not have accsess to a VM, so I set it up on my Macbook, here are the detailed instructions

You are going to need Homebrew, python 3 and pip preinstalled to do this.

First, we have to install the prerequisites
```
brew update
brew install wget unzip

```
Then I proceeded as normal  and ran 
```
$ wget https://terokarvinen.com/hack-n-fix/teros-challenges.zip
$ unzip teros-challenges.zip

```

```
pip install flask
pip install Flask Flask-SQLAlchemy
pip install sqlalchemy
```
After this we can run as normal

My first idea was to just write 'OR 1=1 ;-- but the system required a number, I figured out that you could remove the input check in inspect element

![[Screenshot 2026-01-20 at 20.32.49 1.png]](Screenshot_2026-01-20_at_20.32.49_1.png)

by deleting that section, it no longer checks for input type, allowing us to type whatever we want

However, the return everything sql injection failed, it only returned 'foo'. At this point, I realized that I knew part of the password and could use LIKE
![[Screenshot 2026-01-20 at 20.37.28.png]](Screenshot_2026-01-20_at_20.37.28.png)

This was my final solution, the password I got was 
```
SUPERADMIN%%rootALL-FLAG{Tero-e45f8764675e4463db969473b6d0fcdd}
```
## b)
Initially fixing the code gave me difficulties, I understood the need for parameterization but had not done it in SQLAlchemy 

Looking at guides and documentation (https://hackernoon.com/sqlalchemy-is-a-better-way-to-run-queries ; https://docs.sqlalchemy.org/en/20/faq/sqlexpressions.html), I decided to try modifying the query to 
```
sql = "SELECT password FROM pins WHERE pin=%s;"
```
and the execute statement to
```
res=db.session.execute(text(sql),(pin))
```
But this caused errors, that were stemming from the text function, removing it however broke the code even further.

After many failed attempts, I figured out that the text function handles parameters differently than normal, I then followed the documentation(https://docs.sqlalchemy.org/en/20/core/sqlelement.html#sqlalchemy.sql.expression.text) to write a parameterized query.

My query looked like this
```
sql = "SELECT password FROM pins WHERE pin=:pin;"
```


and my execute statement looked like this

```
res=db.session.execute(text(sql), {"pin": pin})
```

After this, I tested the program, and the injection failed!!!!

![Screenshot 2026-01-20 at 22.01.21.png](Screenshot_2026-01-20_at_22.01.21.png)
## c)
I downloaded the wordlist, dirfuzt-1 and ffuf on my VM, and then opened the page

After running ffuf on it, I noticed that most URL's were 154 bytes large
<img width="654" height="515" alt="image" src="https://github.com/user-attachments/assets/a5a3acd4-d654-4091-bcd4-e6b3751ae1b6" />

I ran ffuf on it with a filter for 154 bytes and got this output
<img width="961" height="573" alt="image" src="https://github.com/user-attachments/assets/04423b15-c925-4b7d-bcc6-ba8bbc81dc1c" />

As wp-admin was the only page that did not seem related to git or google, and as it had a very suspicious name, i checked it first, and found the flag.

<img width="444" height="668" alt="image" src="https://github.com/user-attachments/assets/4ff2f557-ca78-4581-a267-69b3ff5e8fa2" />



(side note, i am logged in as root because i have not added my user to sudo users yet, which I will after this)


## d)
This assignment was easier than 010

After setting up django and disconnecting from the internet. I tested out the website by making a dummy account, after that I ran ffuf and got the correct address(admin-console) fairly quickly.
![[Screenshot 2026-01-20 at 23.28.20.png]](Screenshot_2026-01-20_at_23.28.20.png)
![[Screenshot 2026-01-20 at 23.42.36.png]](Screenshot_2026-01-20_at_23.42.36.png)

## e)
I was quite stuck, until I checked the tips, and realized that permission checking is done in the views.py file

Then I noticed that function in AdminShowAllView was missing checks for permissions that were present in the AdminDashBoardView.



![[Screenshot_2026-01-21_at_16.09.56.png]](Screenshot_2026-01-21_at_16.09.56.png)
I simply copied the test function from there, to AdminShowAllView


![[Screenshot_2026-01-21_at 16.10.12.png]](Screenshot_2026-01-21 at_16.10.12.png)After that, the page showed a 403 error.
![[Pasted image 20260121161343.png]](Pasted_image_20260121161343.png)


## g)

I had solved this in class, but I solved it again here
I observed in the URL that it used string contactenation for executing SQL queries, I then typed in

```
filter?category='OR 1=1 OR ' '='
```

and got access

<img width="2512" height="660" alt="image" src="https://github.com/user-attachments/assets/bd8a43a1-d4ba-4153-b83f-0efe7c76a91e" />

## h)
I first checked what the request contained in the developer console, then changed my password to an SQL injection

<img width="598" height="232" alt="image" src="https://github.com/user-attachments/assets/014e0ff4-d484-443f-868e-d72ac945aadc" />

This request was succesful and I got in

<img width="2280" height="478" alt="image" src="https://github.com/user-attachments/assets/b445997d-97e3-4f8e-8df5-f8529bc2dccd" />
