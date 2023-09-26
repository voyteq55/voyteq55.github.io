---
title: How to Combine Multiple Repositories into One (and Preserve History)
categories: 
tags: [git]
---

## The Why

At the start of my first year of CS education I was still trying to quite grasp my head around the basics of git. For this reason, every small exercise that I did for my introductory OOP class was designated its own repository, so I could play with the tool and familiarize myself with it (or so I thought).

This resulted in me having too many repos cluttering my GitHub and disrupting my inner peace and balance. Something had to be done with this unacceptable mess, I said to myself.

So what I decided to do was to **combine these repositories into one** containing all of them as subdirectories, while also **preserving commit history** of every one of the many original repos. Here I'm going to show how to do it.

We're going to go from this:

```
── project_1
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
── project_2
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
── project_3
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
...
── project_n
   ├── .git
   ├── .gitignore
   ├── some_file.txt
   └── some_directory
```

...to this:

```
── one_big_repository
   ├── .git
   ├── .gitignore
   ├── project_1
   |   ├── some_file.txt
   |   └── some_directory
   ├── project_2
   |   ├── some_file.txt
   |   └── some_directory
   ├── project_3
   |   ├── some_file.txt
   |   └── some_directory
   |   ...
   └── project_n
       ├── some_file.txt
       └── some_directory
```