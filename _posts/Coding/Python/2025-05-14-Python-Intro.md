---
layout: post
title: "Python Intro"
date: 2025-05-14 14:08:00 -0700
categories: [Coding]
tags: [Coding]
description: Learning Python Basics
---

# Learning Python

## Chapter 1 The basics

> `Before Getting Started` 
    This Module will be more informal of my learnings of Python. It will consist more of a note vibe.
{: .prompt-info }

- I will be using Visual Studio Code to run my work.

#### Running hello_world.py

Before getting started, create a folder on your desktop called python_work. This is where you'll keep all your Python projects. Stick to lowercase letters and use underscores instead of spaces—it's the standard style in Python.

Next, open VS Code and close the "Get Started" tab if it pops up. Create a new file by clicking File → New File or pressing CTRL+N (⌘+N on Mac). Save the file as hello_world.py inside your python_work folder. The .py extension tells VS Code that it’s a Python file.

Now, type this into your new file:

```python
print("Hello Python world!")
```

To run your program, go to Run → Run Without Debugging or press CTRL+F5. You should see this in the terminal at the bottom:

**Hello Python world!**

As you write code, your editor (like VS Code) will automatically highlight different parts in different colors. For example, it knows that print() is a function, so it shows it in one color. It also recognizes that "Hello Python world!" is a string (not code), so it highlights it differently.

This is called syntax highlighting, and it helps make your code easier to read and understand—especially when you're just starting out.

##### Variables

message = "Hello Python world!"

print(message)

Result = Hello Python world!

We’ve added a variable named message. Every variable is connected to a value, which is the information associated with that variable. In this case the value is the "Hello Python world!" text.

message = "Hello Python Crash Course world!"

print(message)

You can change the value of a variable in your program at any time, and Python will always keep track of its current value.

##### Naming and Using Variables

When creating variables in Python, there are a few important rules and tips to follow. Some prevent errors, while others just make your code easier to read:

- Use only letters, numbers, and underscores

Variable names can start with a letter or an underscore (_), but not a number.

✅ message_1 — OK

❌ 1_message — Error

- No spaces in variable names

Use underscores instead:

✅ greeting_message

❌ greeting message

- Don’t use Python keywords or function names

Avoid names like print, for, or list — these are built into Python and using 
them can cause problems.

- Keep names short but clear

✅ name is better than n

✅ student_name is better than s_n

✅ name_length is better than length_of_persons_name

Watch out for confusing letters

Lowercase l and uppercase O can look like 1 and 0 — try to avoid using them in variable names.

A string is a series of characters. Anything inside quotes is considered a string in Python, and you can use single or double quotes around your strings like this:

"This is a string."

'This is also a string.'

```python
name = "ada lovelace"
print(name.title())
```

Result = Ada Lovelace

In this example, the variable name refers to the lowercase string "ada lovelace". The method title() appears after the variable in the print() call. A method is an action that Python can perform on a piece of data. The dot (.) after name in name.title() tells Python to make the title() method act on the variable name. Every method is followed by a set of parentheses, because methods often need additional information to do their work. That information is provided inside the parentheses. The title() function doesn’t need any additional information, so its parentheses are empty.

![Desktop View](/assets/img/python/ada_example-1.png){: width="700" height="400" }

In some situations, you’ll want to use a variable’s value inside a string. For example, you might want to use two variables to represent a first name and a last name, respectively, and then combine those values to display someone’s full name:

```python
first_name = 'ada'
last_name = 'lovelace'
full_name = f'{first_name} {last_name}'
print(full_name)
```

To insert a variable’s value into a string, place the letter f immediately before the opening quotation mark. Put braces around the name or names of any variable you want to use inside the string. Python will replace each variable with its value when the string is displayed.

These strings are called f-strings. The f is for format, because Python formats the string by replacing the name of any variable in braces with its value. The output from the previous code is:

ada lovelace

![Desktop View](/assets/img/python/ada_example-2.png){: width="700" height="400" }

In programming, whitespace refers to any nonprinting characters, such as spaces, tabs, and end-of-line symbols. You can use whitespace to organize your output so it’s easier for users to read.

To add a tab to your text, use the character combination \t:

To add a newline in a string, use the character combination \n:

![Desktop View](/assets/img/python/ada_example-3.png){: width="700" height="400" }

You can also combine tabs and newlines in a single string. The string "\n\t" tells Python to move to a new line, and start the next line with a tab. 

To remove the whitespace from the string, you strip the whitespace from the right side of the string and then associate this new value with the original variable ❶. Changing a variable’s value is done often in programming. This is how a variable’s value can be updated as a program is executed or in response to user input.

##### Whitespace

You can also strip whitespace from the left side of a string using the lstrip() method, or from both sides at once using strip():

❶ >>> favorite_language = ' python '

❷ >>> favorite_language.rstrip()
' python'

❸ >>> favorite_language.lstrip()
'python '

❹ >>> favorite_language.strip()
'python'

