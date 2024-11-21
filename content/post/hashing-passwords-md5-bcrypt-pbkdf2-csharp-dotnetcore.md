+++
author = "Simon Gilbert"
categories = ["Algorithm", "C#", "Chief Technology Officer", "C#.Net", "Comparison", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dot Net", ".Net Core", ".Net Framework", "Object Oriented Programming", "Performance", "Simon Gilbert", "Cryptography", "Hashing", "Hacking", "Password", "Passwords", "Password Hashing", "Hashing Algorithms", "Hash Function", "Hash Functions", "Cryptographic Hash Functions", "MD5", "BCrypt", "PBKDF2", "Asp.Net Identity Framework", "Rfc2828DeriveBytes", "Code", "Coding", "Dev", "Programming", "Software Development", "Web Dev", "Web Development"]
date = 2019-03-21T14:53:59Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-ten.png"
slug = "hashing-passwords-md5-bcrypt-pbkdf2-csharp-dotnetcore"
summary = "Need to cryptographically hash your users passwords? Simon Gilbert discusses the potential issues with the MD5 hash algorithm vs. BCrypt and PBKDF2, including the logic behind brute force attacks and correct cryptographic approaches in C# .Net Core."
title = "Hashing Passwords Using The MD5 Hash Algorithm vs. BCrypt vs. PBKDF2 - C# .Net Core"
tags = ["C#.Net", "Cryptography", "MD5", "BCrypt", "PBKDF2"]
+++


### "Please Enter Your Password"

...You've just signed up to a new website and as per usual, they require you to create an account using your email address and a password. This is all well and good...assuming the password isn't stored in plain text for anyone with database access to read/borrow/use/compromise...hack? Cryptography exists for a reason, although it isn't always implemented using the best approach...

### Encryption vs. Hashing

Encryption is a 1:1 mapping and is always reversible through decrypting. Hashing is one-way and cannot be reversed. If you encrypt plain text with a key, you will get the exact plain text back when you decrypt it. If you hash plain text with a key, you cannot get the same plain text value back if you attempt to reverse it, hence "one-way".

### Cryptographic Hash Functions

...To solve the plaintext password problem, **one-way hashing algorithms** are used to generate a different output for the password. These are called **"cryptographic hash functions"**, and their purpose is to map the password to a bit string of a fixed size (known as the **hash**).

The output of this mathematical algorithm is called the **hash digest**, and this is what is stored in a database instead of storing your password in plaintext. In future when you are asked to log in, the password you enter is re-hashed using the same algorithm, and the hash digest that is produced is compared with the existing stored hash digest. If they match, you're logged in successfully.

### Password Hashing - Process Flow Summary

1) Sign up to www.somewebsite.com using a password.

2) www.somewebsite.com hashes your password using a hash function, and stores the hash digest.

3) Log in to www.somewebsite.com by entering said password, which is hashed again upon entering it, using the same hash function as used at sign up, and the hash digests are compared.

4) Assuming the hash digests match, you're logged in successfully...

### Hash Collision (Attacks)

Due to the design of a hash function (infinite input length, predefined output length), there is indeed the potential that two different inputs produce the same hash output. This is known as a hash collision, and can be exploited by hackers in scenarios where an application compares two hashes together and performs a task following a successful match...such as logging a user into their personal account on a website.

A commonly used addition to the hashing process is to **"salt"** the passwords, whereby a known random string is appended to the original string before it is injected into the hashing function.

### MD5 Password Hashing

One of the most popular cryptographic hash functions is called "**MD5**", which tends to be used because people think they should use it, are used to using it...and people don't like change...the problem is, it's weak, and I'm not necessarily talking cryptographically here...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-md5-cryptography-hash-function.png" caption="MD5 Password Hash Algorithm - C# .Net Core" >}}

### Rainbow Tables

A rainbow table is used to reverse cryptographic hash functions. It's quite literally a look up table that contains all the variations of characters that have been hashed, using a specific hashing algorithm, such as **MD5**. It wouldn't take long to brute force a series of **MD5** passwords using a rainbow table...

...So, how to avoid this problem? ...You can "**salt**" your passwords as mentioned earlier, although that in itself doesn't necessarily answer the question...

### Salted MD5 Passwords vs. Brute Force Attack

Ignoring the cryptographic weaknesses of **MD5** for a minute, let's talk about speed. By choosing to use a fast hash construction, you are consequently allowing a hacker to fire millions of "candidate" passwords per second in an attempt to obtain a match, and all they need is a single GPU to do it...Modern computational power has increased so much over the years and is readily available. Your biggest enemy when it comes to password hashing is **TIME!**

[._..Here's a list fo RECENT MD5 breaches_](https://www.theregister.co.uk/2019/02/11/620_million_hacked_accounts_dark_web/)

### Thinking of Migrating to SHA-1 or SHA-2?

So **MD5** isn't ideal...but of course you don't have to use MD5...**SHA**-**1** (Secure Hash Algorithm **1**) produces a 160-bit (20-byte) hash value, typically rendered as a hexadecimal number, 40 digits long.

Even better...**SHA-2** was designed by the National Security Agency (NSA)...They are built using the Merkle–Damgård structure, from a one-way compression function which itself is built using the Davies–Meyer structure from a (classified) specialised block cipher!

Sounds great right? ...Rainbow tables apply almost equally to all unsalted hashes. Therefore migrating to **SHA-1** or SHA-2 would do almost nothing.

### The Correct Approach

When it comes to hashing passwords, the correct approach is to choose a deliberately slow cryptographic hashing function combined with a salt. Time is of the essence...and the more time it takes, the less implied a hacker will be to monotonously brute force your log in functionality - A process which is easier than it seems if your're using **MD5**, **SHA-1** or **SHA-2**...

### Blowfish To The Rescue?

In 1999, the **BCrypt** hashing function was designed and is based on the Blowfish cipher. To cut a long story short, it incorporates a salt and is an adaptive function, meaning it can be slowed down over time to help prevent against brute force attacks. It's interesting to think that the designers purposely put performance control into this hash function in order to circumvent potential attacks...But before you get started with the migration from **MD5**...let's talk about a monumental hack involving **BCrypt**...

### The Ashley Madison BCrypt Hack

Remember that website which hit the papers in 2015? It was known for helping to enable extramarital affairs happen (grim), and was called "Ashley Madison". Well, their users passwords were all stored using **BCrypt**, and in some cases due to a major design error they were stored using **BOTH** MD5 (originally, pre 2012) and then BCrypt (from 2012 onwards, after their developers made a design amend but fundamentally made a few mistakes involving the generation of the $loginkey).

What's more is that the developers at Ashley Madison used a cost factor of 12 for their implementation of **BCrypt**, which ultimately meant that each "candidate" password that an hacker wanted to test in a brute force attack would need to go through 4,096 rounds of hashing. This decision ensured that each password guess was in fact potentially thousands of times slower to perform than an **MD5** brute force attack...

...and still it got breached, monumentally, to the point where the most common passwords used were in fact "123456" and "password". The total exploited password count was 36 MILLION!! Realistically the design change was to blame, as was the blunder from the dev team, but it made an interesting headline too!

### Enter "PBKDF2"...

**PBKDF2**, known as Password-Based Key Derivation Function 2 uses a pseudorandom function and a configurable number of iterations to derive a cryptographic key from a password.  Key derivation functions are ideally suited for password hashing use cases, and similarly to **BCrypt** it uses a large random "salt" value to ensure that each password is hashed uniquely. Due to it's design it is cryptographically slow to compute on purpose and as mentioned before, time is the enemy in a brute force attack. **PBKDF2** prevents password cracking tools from making optimal use of graphics processing units (GPUs) and massively increases the required guess rates that a hacker would have to make.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-pbkdf2-cryptography-hash-function.png" caption="PBKDF2 Password Hash Algorithm - C# .Net Core" >}}

### ASP.Net Identity Framework Cryptography

...What's the chance that you're a C# .Net developer? Perhaps you've used **ASP.Net Identity Framework** for authentication and password hashing? Fortunately for you, Microsoft chose to use **Rfc2898DeriveBytes** for ASP.Net and this class implements **PBKDF2** . To be honest it should have been named **PBKDF2DeriveBytes**...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-password-hash-outputs.png" caption="MD5 Hash vs. PBKDF2 Hash - C# .Net Core" >}}

### In Summary...

Avoid MD5...**PBKDF2** is supported in .Net out of the box and cryptography isn't something to be taken lightly, especially at enterprise level. Troy Hunts infamous **[HaveIBeenPwned](https://haveibeenpwned.com/)** is worth signing up to, if you haven't already...

Enjoy!