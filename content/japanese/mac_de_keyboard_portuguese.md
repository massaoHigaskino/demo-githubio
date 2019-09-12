---
title: "Using a German Mac keyboard to write in Portuguese on Linux Mint"
date: 2019-09-12T11:23:06+01:00
draft: true
---

As the long title says, I have a very specific case to show. I have got a German MacBook Pro and frequently need to write in Portuguese. It works well under macOS. However, the default keyboard layout on Linux Mint is not quite the same (titled ``Deutsch (Macintosh)``). This makes quite difficulty to type characters with tilde (e.g. ã, õ) and cedillas (e.g ç). Along this post I will show you how to change the ``Deutsch (Macintosh)`` keyboard layout to allow you to type those characters.

The keyboard layouts on Linux Mint are defined by text files located in ``/usr/share/X11/xkb/symbols`` and every German layout is defined inside the ``de`` file. Choose your preferred text editor and open it (you will need ``root`` privileges to edit it).

Since you are going to edit a configuration file, it is a good idea to make a backup of it.

Inside the file you will come across the following block:

```
xkb_symbols "mac" {
    ...
};
```

You will need to edit one existing line, and add another one.

First, find the following line:

```
key <AB06> { [ n, N, asciitilde] };
```

This line is telling that using alt+n will result in a tilde (~), but not a dead key one, which won't allow you to use it to modify another character. Change it into the following:

```
key <AB06> { [ n, N, dead_tilde] };
```

Now you will be able to use alt+n to add tildes to other characters.

Second, we will need to add a way to type ç and Ç, which is as simple as adding the following line inside the block:

```
key <AB03> { [ c, C, ccedilla, Ccedilla] };
```

Now you are ready to go. Save your changes, logout from your current session, login again, and test your keyboard.