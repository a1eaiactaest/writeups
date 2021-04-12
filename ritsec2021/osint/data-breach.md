> # Data Breach
> ## Points 175
> Oh no! Geno’s email was involved in a data breach! What was his password? 
> 
> Author: t0uc4n

## Solution

First we need to know what was Geno's email.  
He didn't add it to contact info on linkedin, but he linked [about.me](http://about.me/genoikonomov) page.  
We can find there links to his social media accounts. On his [github](https://github.com/incogeno) page we can see his email which is:

```
incogeno@gmail.com
```

Using [](https://haveibeenpwned.com/) doesn't show anything useful.
Having email we can use [google dorking](https://medium.com/infosec/exploring-google-hacking-techniques-using-google-dork-6df5d79796cf) to search for data leaks containing his email.   

Looking this up:

```
allintext: incogeno@gmail.com
```

We are prompted with [this](https://nss.ackaria.xyz/index.html) data breach.
Quick `ctrl+f` to search for Geno's email and we have results:

```
email=incogeno@gmail.com:password=StartedFromTheBottom! 
```

So flag is: `RS{StartedFromTheBottom!}`
