# PicoCTF - General Skills: Fixme1.py

## Description

The challenge provides a Python script containing a syntax/indentation error. The goal is to fix the script so that it runs correctly and reveals the flag.

## Solution

First, I opened the provided Python file using Nano:

```bash
nano fixme1.py
```

After inspecting the code, I ran the script to identify the error:

```bash
python3 fixme1.py
```

The program returned the following error:

```text
File "/home/elok/Downloads/fixme1.py", line 20
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```

The error message indicated that there was an unexpected indentation on line 20. Since Python relies on indentation to define code blocks, an extra space or tab can cause the interpreter to fail.

I reopened the file and corrected the indentation issue by removing the unnecessary indent before the `print()` statement.

```bash
nano fixme1.py
```

After saving the file, I executed the script again:

```bash
python3 fixme1.py
```

This time the script ran successfully and printed the flag.

## Flag

```text
picoCTF{1nd3nt1ty_cr1515_182342f7}
```
