# Digital_System_Design: Booth Multiplication

## Introduction:

Our objective is to create a booth multiplication circuit that takes two 5-bit numbers (Say R &M) and output their multiplied form. Here a step by step explanation of theoretical processes that are used to implement the circuit is given. 

One point to remember is that we will only be using numbers represented with 4 bits. 5th bit will be the sign bit.

![Rules](https://user-images.githubusercontent.com/43475529/63374605-f93f2600-c3ab-11e9-9e44-4a639470b3d0.png)

![Sample](https://user-images.githubusercontent.com/43475529/63374641-0cea8c80-c3ac-11e9-8b49-d73687fa71e3.png)

![Steps](https://user-images.githubusercontent.com/43475529/63374661-183db800-c3ac-11e9-955d-03af54192d6f.png)

[Note: ">>" symbol means right shift as the operator is used in bitwise operations. P>>n means right shifting P n times.]

After removing the last bit from P we get our desired answer.
So, Answer: 00000 01100 --> 12 = 4*3

