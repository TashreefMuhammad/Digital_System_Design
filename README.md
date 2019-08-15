# Digital_System_Design: Arithmetic & Logic Unit

### We are to implement a 4 bit ALU that takes two 4 bit numbers A(A3,A2,A1,A0) & B(B3,B2,B1,B0) and has S2, S1 & S0 as three selection switches to implement the given truth table

The given Truth table to be implemented was :
![Given_Table](https://user-images.githubusercontent.com/43475529/63110107-83eae400-bfac-11e9-8d3b-86230a93f3c4.png)

We assumed X, Y & Z as the functions that will enter ALU as input (Z is one bit, carry in, others are 4 bit each) & a 4 bit output number F (F3,F2,F1,F0) along with a carry out bit will be shown as end result.

As so, our truth table for implementing the ALU is:
![First_Stage_Level](https://user-images.githubusercontent.com/43475529/63110044-6453bb80-bfac-11e9-8052-c0b78965ed13.png)

So, to implement a K-Map through which we shall get the functions of X, Y & Z we need the broad table as below:
![Elaborate_Truth_Table](https://user-images.githubusercontent.com/43475529/63110016-5140eb80-bfac-11e9-9e5a-4faad100c712.png)

We can see X,Y & Z all need S2, S1 & S0 as variables. In addition, X needs A & B, Y needs B also as variables.

So,
For X we need 5 variable K-Map
For Y we need 4 variable K-Map
For Z we need 3 variable K_Map

They are as follows:

### Z:
K-Map for Z,

![Z_K-Map](https://user-images.githubusercontent.com/43475529/63110184-a54bd000-bfac-11e9-84fc-a36c869fb12d.png)

![Z_Func](https://user-images.githubusercontent.com/43475529/63113025-7d139f80-bfb3-11e9-87ca-b70f5472bb6f.png)


### Y:
K-Map for Y

![Y_K-Map](https://user-images.githubusercontent.com/43475529/63110160-9a913b00-bfac-11e9-8d10-d056f71497e3.png)

![Y_Func](https://user-images.githubusercontent.com/43475529/63112964-56556900-bfb3-11e9-8a90-167f810fe3cf.png)

### X:
K-Map for X,

![X_K-Map](https://user-images.githubusercontent.com/43475529/63112865-1d1cf900-bfb3-11e9-80bd-1f2db93addcd.png)
![X_Func](https://user-images.githubusercontent.com/43475529/63112909-3625aa00-bfb3-11e9-8859-64d57b5c2690.png)

And finally we implement the function. The simulation is given above using Proteus 8.0
