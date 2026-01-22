

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

![[Screenshot 2026-01-20 at 20.32.49 1.png]](Screenshot 2026-01-20 at 20.32.49 1.png)

by deleting that section, it no longer checks for input type, allowing us to type whatever we want

However, the return everything sql injection failed, it only returned 'foo'. At this point, I realized that I knew part of the password and could use LIKE
![[Screenshot 2026-01-20 at 20.37.28.png]](Screenshot 2026-01-20 at 20.37.28.png)

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

![[Screenshot 2026-01-20 at 22.01.21.png]](Screenshot 2026-01-20 at 22.01.21.png)
## c)


## d)
This assignment was easier than 010

After setting up django and disconnecting from the internet. I tested out the website by making a dummy account, after that I ran ffuf and got the correct address(admin-console) fairly quickly.
![[Screenshot 2026-01-20 at 23.28.20.png]](Screenshot 2026-01-20 at 23.28.20.png)
![[Screenshot 2026-01-20 at 23.42.36.png]](Screenshot 2026-01-20 at 23.42.36.png)

e)
I was quite stuck, until I checked the tips, and realized that permission checking is done in the views.py file

Then I noticed that function in AdminShowAllView was missing checks for permissions that were present in the AdminDashBoardView.



![[Screenshot 2026-01-21 at 16.09.56.png]](Screenshot 2026-01-21 at 16.09.56.png)
I simply copied the test function from there, to AdminShowAllView

![[Screenshot 2026-01-21 at 16.10.12.png]](Screenshot 2026-01-21 at 16.10.12.png)After that, the page showed a 403 error.
![[Pasted image 20260121161343.png]](Pasted image 20260121161343.png)


