# PicoCTF - General Skills: Fixme2.py

## Description

This challenge provides a Python script containing a syntax error. The objective is to identify and fix the error so the script can execute correctly and reveal the flag.

## Solution

I started by running the provided script:

```bash
python3 fixme2.py
```

Python returned the following error:

```text
File "/home/elok/Downloads/fixme2.py", line 22
    if flag = "":
       ^^^^^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

The error message indicated that the condition inside the `if` statement used the assignment operator (`=`) instead of the equality comparison operator (`==`).

In Python:

* `=` is used for assignment.
* `==` is used to compare two values.

The problematic line was:

```python
if flag = "":
```

I edited the file using Nano:

```bash
nano fixme2.py
```

Then I corrected the line to:

```python
if flag == "":
```

After saving the changes, I ran the script again:

```bash
python3 fixme2.py
```

The program executed successfully and displayed the flag.

## Flag

```text
picoCTF{3qu4l1ty_n0t_4551gnm3nt_f6a5aefc}
```
