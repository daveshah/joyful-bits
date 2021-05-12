---
title: SQL'ing Safely
date: "2021-05-21"
---

Running a SQL script in production can be a bit scary. I think it goes without saying that it should probably be avoided more often than not. BUT, like all things, context matters.
You may find yourself in a particular situation where running a SQL script in production is the optimal choice - and that's okay. If you're in that situation, it can feel a bit scary. After all, it *is* production.

Years ago, I worked on a team with a really talented developer named Grant Hughes. Grant had a really clever pattern he followed and that I adopted with a bit of modification. 
It's a simple pattern and it's helped me feel much more at ease when running any sort of SQL scripts in production. 

It goes something like this:
```
BEGIN TRANSACTION;
-- Inspect (SELECT ...)

-- Act (INSERT, UPDATE, ...)

-- Assert (SELECT ...)
ROLLBACK TRANSACTION;
```

Look familiar? It's similar to the [arrange act assert pattern](http://wiki.c2.com/?**ArrangeActAssert) for formatting unit tests.
Instead of *arrange*, it begins with *inspect* - inspecting the state of the existing data. This often takes the form of a `SELECT` statement from which you can visually inspect the current state.
It's followed by *act* - the action you intend to take. This is often that `INSERT` or `UPDATE` statement you've written (that thing that feels risky in production)!
Finally, we *assert* (really just another inspect). This will often be another `SELECT` statement where we can assert that our action in the previous section had the intended outcome.

The part of this pattern that I love most: This is tis all wrapped within a transaction that's **rolled back**.

This is the part that helps in providing that warm and fuzzy feeling you might be seeking out before just running your SQL in production. 

If you can feel comfortable with the output of the assert section, you can simply change that rollback to a `COMMIT` once you're ready.

As you can see, it's a pretty simple pattern and it's definitely saved me more than one time. 

If you use a SQL client / editor that supports any sort of canned templates when you open your editor - this one may come in handy 😁

Special thanks to my teammate Dan Meehan for the inspiration to finally blog about this. Dan wanted to run an approach by me this afternoon and I ended up sharing this story with him for the first time. Doing so reminded me that this is one of those small but potentially helpful things that I keep on sharing in verbal form but haven't taken the time to write down yet. Now it's here so I can stop repeating myself 🤣 

I hope this helps!  

~ Dave



