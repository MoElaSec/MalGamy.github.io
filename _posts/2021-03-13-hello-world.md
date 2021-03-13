---
title: CSAW CTF 2014 -- Exploitation 400 saturn
layout: single
comments: true
share: true
related: true
author_profile: true
permalink: "/:title/"
tags:
- CTF
- CSAW
- Python
- Pwnable
categories:
- write-ups
date: '2014-09-25 09:35:00 +0000'
---

First the challenge gave us a binary file (ELF for Intel-386). But we can't execute it, cause we don't have the required shared library **"libchallengeresponse.so"**. So we will have to launch IDA Pro to see what's going on within the program.
<!-- more -->

After analyzing the program ( praise the powerful F5 key! ) , I collected the following information:

# Main function

``` c
int main()
{
  puts("CSAW ChallengeResponseAuthenticationProtocol Flag Storage");
  
  for ( i = 0; i <= 0; ++i )
  {
    v0 = read_one_byte();
    v3 = v0;
    v1 = v0 & 0xF0;
    switch ( v1 )
    {
      case 160: //0xA0
        functionA(v3);
        i -= 2;
        break;
      case 224: //0xE0
        functionB(v3);
        i -= 2;
        break;
      case 128: //0x80
        functionC();
        break;
    }
  }
  return 0;
}
```

As we can see, the main function will read a byte from the user input, and do the `&` operation. `v1` will hold the result, and a `switch-case` condition will decide which function to run next, base on `v1`'s value. 

So basically, we can summarize that the main function will **only accept 3 kinds of input**: `0xA0 ~ 0xAF`, `0xE0 ~ 0xEF` and `0x80 ~ 0x8F`. Now let's take a good look at those functions.

# functionA
