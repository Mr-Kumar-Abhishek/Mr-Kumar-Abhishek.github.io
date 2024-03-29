---
layout: post
title: "Is it necessary to include return 0; in main function of C++ ?"
description: We should include return 0; in main function of a C++ program as per the standard but no, it is not necessary.
subject: Should we include return 0 in C++ main function?
apple-title:  is it necessary to include return 0?
app-name: Should we include return 0 in C++ main function?
tweet-title: Is it necessary to include return 0; in main function of C++ ?
tweet-description: We should include return 0; in main function of a C++ program as per the standard but no, it is not necessary.
og-title: Should we include return 0 in C++ main function?
date: 2022-08-31
keywords: Abhishek Kumar, Software Developer, C++, main, main function, return 0, necessity
---

The inclusion of `return 0;` in `main` function of a typical C++ program is mentioned by the [C++ standard](https://isocpp.org/std/the-standard). It is there to indicate that there was successful completion of the program.
We should keep it in `main` program of C++ regardless if we do something else or not in the program:

```
#include <iostream>

int main() {
	return 0;
}
```

You may now ask, where the `0` value is returned to? Well, it is returned to the OS. The OS will be expecting a program to return `0` if everything went correctly and smoothly. Whereas if it gets a non-0 value, it understands that the program has got errors.

However, C++ allows you to exit the `main` function without explicitly returning anything. 

```
#include <iostream>

int main() { }
```

Here, the compiler is required to automatically insert a `return 0;` in the `main` function.  But, keep in mind that every other non-void function is required to return something. If it doesn't, it flags unpredictable and subtle errors.

### Conclusion

**We should `return 0;` in `main` function of a C++ program as per the [standard](https://isocpp.org/std/the-standard) but no, it is not necessary.**

More details on writing `main` function can be found [here](https://mr-kumar-abhishek.github.io/blog/2020/04/11/overview-of-main-function-in-c-and-c-plus-plus).
