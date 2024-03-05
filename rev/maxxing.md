# maxxing [solved]

***


## Solution 


we have an executable `minimax`\
which on execution is a simple tic tac toe game\
where it's impossible to win against the bot.

opened it up on IDA and started going through it and found a function `syaaoksnqd`.\
All the juice seemed to be happening here.\
there was even a fake flag in here:

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/14bc372b-8270-4a94-b682-8baa03809aac)


I saw a lot of string concatenation

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/35e5a48e-a819-41fe-b5e4-3bc9272e8b46)

seemed to be a hex code.


[i initially opened it with ghidra, but I switched to IDA halfway because Rupak.]\
[he told me that ghidra displays hex content from the other end (its apparently called swapped endianness I learned)]

I got this as the hex code:\
`0x7008761D1E0CC30311AA0A04BD5D44A9F36233921865AE9DD4D471F06298B2FD94F`

did unhexify it, but it gave pure bullshite.

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/ff1583ec-6962-4c94-9fb1-15e58cf01708)

I also found two pointless numbers, that were just there:

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/453cc22b-0e68-476b-9d23-4a4beeedc8b3)

no idea what to do with these, probably a red herring.\
Nearing the end of the ctf, Aakash got a hint:

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/513a970c-483c-48ba-b585-d1941214bc1f)

I knew which for loop it was talking about.\
I hadn't thought much of it, thinking it was probably one of those random thingys of a decompiled binary\
here's the loop : 

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/bbf76dca-e929-4999-b65c-ff26cb38edc4)

The loop was traversing an array,\
and multiplying with each element of array.\
It took me a second but I did figured it out.\
It was pretty guessy ngl.

you had to convert the hex to long, and then put that long along with the two other numbers into that for loop.\
basically multiply all three.

```python
#bruh 

from Crypto.Util.number import *

a = int("0x7008761D1E0CC30311AA0A04BD5D44A9F36233921865AE9DD4D471F06298B2FD94F",16)
b = 307
c = 132440257

result = a*b*c

flag = long_to_bytes(result)
print(flag)
```

![image](https://github.com/IC3lemon/BITSCTF-2024/assets/150153966/ab17268d-7729-4ebd-aaf2-3b290afdb25b)

I was so happy and headed back to discord, just to see that lepnoxic solved the shit 20 minutes ago ;-;\
Oh well...

***

## Flag > `BITSCTF{w3_n33d_t0_st4rT_l0ok5m4Xx1nG}`

***
