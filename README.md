# Digital_System_Design: Simple As Possible (SAP) - 1

### We are to design a SAP-1 that can do 5 operations that will be described later on. As this is going to be a bit long explanation, all the parts are going to be divided.

### The parts are as follows:
1. [General Design of SAP-1](#general-design-of-sap-1)
2. [Description in detail of each component](#description-in-detail-of-each-component)
    - [Program Counter](#program-counter)
    - [MAR (Memory Address Register)](#mar)
    - [16x8 RAM](#16x8-ram)
    - [Instruction Register](#instruction-register)
    - [Control Sequence](#control-sequence)
    - [W-Bus](#w-bus)
    - [Accumulator A](#accumulator-a)
    - [Adder/Subtractor](#adder/subtractor)
    - [Register B & Register Output](#register-b-&-register-output)
    - [Colck (CLK)](#clock)
    - [Clear (CLR)](#clear)
3. Input Manipulation of SAP-1
5. Step Guide

### General Design of SAP-1:
A SAP-1 is capable of doing 5 operations. The commands can be written like:

***Opcode Location***

Where *Opcode* denotes the operation name, *Location* denotes the memory address from which data shall be fetched. The 5 type of commands that a SAP-1 can process are:
<pre>
1. LOAD (Loading data to A register)
2. ADD  (Adding data of A & B register and storing it in A register)
3. SUB  (Subtracting data of B from A register and storing it in A register)
4. OUT  (Output the value stored in A register)
5. HLT  (Hault command to stop the SAP-1 to moving on forward anymore)
</pre>
A set of 4 bits together are used to form the opcode. In usual or reknowned cases the famous sequence for the opcodes are:
<pre>
LOAD  -> 0000
ADD   -> 0001
SUB   -> 0010
OUT   -> 1110
HLT   -> 1111
</pre>
However, I have a different idea. I will be changing this opcode pattern to my liking, causeusually everyone follow the above pattern (and also cause I figured a small shortcut for my control sequences if I change the bits a bit :-P). So, here the sequence that will be used are:
<pre>
LOAD  -> 0000
ADD   -> 0001
SUB   -> 0011
OUT   -> 0100
HLT   -> 1000
</pre>
The general design without control words (Just the components) of SAP-1 can be shows like as follows:

![OverLay](https://user-images.githubusercontent.com/43475529/67584556-bf095080-f76f-11e9-82f0-848df3b97437.png)

The 12 bit control word will be explained later and they are very important for step controlling of SAP-1. Another important thing to remember is that though in all theoritical process, there is always written to be 12 control bits, we will be using another additional bit (In total 13 bits). The additional bit will help stop the SAP-1 when we order a **HLT** operation.

### Description in detail of each component:
Each of the components are described point by point.
#### *Program Counter:*
The work of program counter has only one work. That is to **count**. When Cp is active and clock pulse is received, we simply need to increase the value in PC (Program Counter) by 1. Here, I put on a JK asynchronous counter circuit to do that job.
However, we can't always let the value of counter circuit to pass through (to W-Bus). The value should be only sent to W-Bus when Ep is set to 1. We control that using tristate buffer. The tristate buffer has one input, one output and one enable pin. When enable pin is activated, value is passed from input to output, or else not. So, we connect Ep to enable pin of tristate buffer.
Hence, the PC should look like:
![SAP-1_Modified_Page_2](https://user-images.githubusercontent.com/43475529/67591089-6ab99d00-f77e-11e9-9603-a1774b800753.png)
#### *MAR:*
The memory address register has a simple job actually. When Lm' is reset to 0, it loads value from W-Bus to input, and when received a clock pulse, sends it through to the 16x8 RAM. Familiar with something like that? It works just like a D-flip flop. As there are 4 bits, we will use a 4 bit register circuit, works the same.
#### *16x8 RAM:*
What does this component do? It stores specific data in specific memory locations. So we are going to use 2732 IC which is a NMOS 32K (4K x 8) UV EPROM. How to store data in specific memory location? That's easy. This portion has but 1 work, when CE' is reset to 0, pass the values from it to W-Bus.
#### *Instruction Register:*
When Li' resets to 0, IR (Instruction Register) takes value from W-Bus and separates them into 2 parts, LSB and MSB 4 bits. When Ei' is reset to 0 then,

i.  LSB 4 bits are sent back to W-Bus

ii. MSB 4 bits are sent to Control Sequence
![SAP-1_Modified_Page_3](https://user-images.githubusercontent.com/43475529/67592598-f41e9e80-f781-11e9-9f76-c6a1a958ad14.jpg)

Once again the data storing and sending is controlled using register same as [MAR](#mar).

#### *Control Sequence:*
In the control sequence we implement functions. As per instructions given. In regular concepts, we can say that SAP-1 follows the following sequence,

![Table](https://user-images.githubusercontent.com/43475529/67594844-0bac5600-f787-11e9-9f60-77e6269260c2.png)

Control sequence also controls T-states. We use a 6 bit D-flip flop ring counter to denote all the 6 T-states.
Recall the opcode bit sequences I declared at [General Design of SAP-1](#general-design-of-sap-1). I denote the 4 bits sent from IR to be I7, I6, I5, I4 from MSB to LSB. From these info, we can say,
<pre>
1.  Cp  = T2
2.  Ep  = T1
3.  Lm' = (T1 + (T4)(I6'))'
4.  CE' = (T3 + (T5)(I6'))'
5.  Li' = T3
6.  Ei' = ((T4)(I6'))'
7.  La' = ((I6')((I4')(T5)+(I4)(T6)))'
8.  Ea  = (T4)(I6)
9.  Su  = (T6)(I4)(I5)
10. Eu  = (T6)(I4)
11. Lb' = ((T5)(I6')(I4))'
12. Lo' = ((T4)(I6))'
13. HLT'= I7 (The additional control bit as mentioned at last paragraph of General Design of SAP-1 above)
</pre>
![ControlSequence](https://user-images.githubusercontent.com/43475529/67597882-07d00200-f78e-11e9-95e2-699a31972f91.png)

#### *W-Bus:*
A simple bus where 8 parallel wires exist. It works just like a simple bus, passes data from one place to another.
#### *Accumulator A:*
Accumulator is used to store run time data. It works in the same principle as [MAR](#mar) with it's own control bits. Differences are that, it has 8 bits (so need 2 registers) and it's ouput to W-bus is controlled using tristate buffers (Just like [Program Counter](#program-counter)).
#### *Adder/Subtractor:*
The design is just like any 8 bit switch control adder/subtractor circuit. Control bit Su works like the switch to control addition & subtraction. Tristate buffers control when value from the adder/subtractor goes to W-bus.
#### *Register B & Register Output:*
Both of them are just the same as [Accumulator A](#accumulator-a) except the tristate buffer blocking the output.
#### *Clock:*
The clock is used for providing clock pulse to the circuit.
#### *Clear:*
Clear sets all the values in circuit and let's the process start anew.
