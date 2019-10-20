---
title: "MacOS Crackme | Sandwich.app"
date: 2019-09-29T13:57:21-07:00

tags: ['macOS', 'CrackMe']
author: "shellcromancer"
draft: true
---

## The CrackMe
I'm gonna walk through my steps in analyzing the macOS CrackMe Sandwich.app made by [@osxreverser](https://twitter.com/osxreverser) and can be found here: https://reverse.put.as/wp-content/uploads/2010/05/1-Sandwich.zip

Our goal is first to reverse the app and see how it works and see if we can get to the success page, and then after that we will patch the app to get us there no matter how wrong we are.

## Reversing Sandwich.app

I started by running the app and seeing its behavior as I punched in a random serial. Every time I got the wrong serial it would pop up an alert saying "The serial is not valid.", lets search the binary for this string.

The function validate (@ 0x1d0e) is where this string is loaded and used to launch a `NSRunAlertPanel`, and this function also prints the success message when the serial is valid. What determines the message printed? If the result of `validateSerial` is 0x0 of 0x1 => the result is true or false. Lets dive into `validateSerial` now!

validateSerial(string serial) takes in a string value and performs a bunch of checks and in most cases will jump to 0x1cd2 where is loads 0 into eax before returning. Reading through the assembly aiming to avoid this jump, lets write out the requirements for our serial string:

1. len(serial) == 0x13 == 19
2. serial is four strings of length 4, seperated by a hypen: `-`, so `aaaa-bbbb-cccc-dddd`
3. Arithmetic requirements: `0x19c5 - RightShift((int(aaaa) + int(bbbb)) == int(dddd)`

Using 1111 for aaaa and 2222 for bbbb we see that dddd is 5764. Trying this in Sandwich is successful! 

![Sandwich Success](/img/sandwich-success.png)

Using this expression in python get valid codes when changing the first two!
```python
first = 1111
second = 2222
third = 0x19c5 - ((first + second) >> 2)
print("{}-{}-xxxx-{}".format(first, second, third))
```

## Patching Sandwich.app
The goal with binary patching, or with this application is so we can change the behavior of the application with our modifications. With this CrackMe the goal is to get the success messsage regardless of the serial code we enter.

In the function `validateSerial` every error condition jumps to the address `0x1cd2`, this is where the return value of false (0) is put into `eax`.

![Sandwich 0x1cd2](/img/sandwich-where-to-patch.png)

It would be sure convient if the return value wasn't 0. So let's patch it out. All we do to do is replace this with a `nop` instruction, which has the opcode `0x90`. 

![Sandwich Patched](/img/sandwich-patched.png)

Any runs, regardless of the input serial, will now give us success. Our job is done now!

![Sandwich Patched Success](/img/sandwich-patched-success.png)
