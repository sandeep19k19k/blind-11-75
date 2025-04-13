# blind-11-75
Sum of two integers

Basically we were asked to add 2 integers without using +,-  
we can implement  the same behaviour using bitwise operations .
There are actually some basic algorithms taught in COA course , on how to deal with this , low level/ machine level implementation.

Bitwise operators Kind of improve the performance drastically.

Now , if the given integers were both positive , this question would be extremely simple.
All we had to do was,
def add_two(a,b):
      while b!=0:
          carry=a&b
           a=a^b
           b = carry<<1
      return a

Basically, carry is a variable that indicates the positions at which both 'a' and 'b' are set to binary '1'.
In all other places using xor ^ operator , addition without carry b/w 'a' and 'b' is carried out and stored in variable 'a'. 
In general carry is pushed to the left adjacent place in normal addition. So we use << left shift operator on carry variable and store that in variable 'b'.
until this variable 'b' is not equal to zero, which means carry is not equal to zero, we keep looping the same thing, while adding carry i.e 'b' to 'a' . 
i.e 1) use & and operator to find carry.
     2)  use ^ xor operator to find sum without carry . 
     3) left shift << carry variable.
     4) add left shifted carry to the sum calculated using xor ^ variable, to get actual sum.

All this is good, until we are working on positive integers.
Now that negative integers are also allowed , as per the given constraints, the logic becomes little complicated because of 2-3 things.
1) The way how python handles integers is different to other languages.
  i.e size is arbitrarily large. It grows as and when required. No fixed size as such. whereas other languages such as C,C++ have fixed size width of 32 bit/ 64 bits.
2) So , we try to simulate that concept using MASK variable . 
 i.e assume we are dealing with 32 bit integers. 
setting MASK= 0xFFFFFFFF (HEXA DECIMAL NOTATION) . 
That is here all bits are set to '1'  .
whenever we do 'a' & MASK , we are indirectly setting all the extra bits(if at all ,beyond the 32 bit size in variable 'a' ) to binary '0' . i.e discard extra information beyond 32 bit size . Simulation of  the 32 bit integer size behaviour in python. 
3) now coming to main concept, dealing with negative integers .
MAX_INT= 0x7FFFFFFF . This is max positive value that can be stored in 32 bit size.
 Anything beyond this is treated as negative in all other languages.
(first bit being 1 , is general indication of the integer being a negative value) And the value of that negative integer is found using 2's complement method.
But in python there are huge no.of zero beyond 32nd bit , it is still treated as positive value and hence ,we need to work around a trick to invoke 2's complement behaviour in python , bcoz only then we can find the value of negative integer.
Here comes the main complement operator '~' .  ( it changes 'n'  to '-n-1' . But that's formula and here much more analysis needed. )  
What does it do? complement each bit. 
yes , by this we can set all those huge no.of binary zero's beyond that 32nd bit to 1 . But aren't we also changing the answer?

Yes, that's why before applying '~' ,
 we perform 'a' ^ MASK. i.e what does this do?
 complements only 32 bits . how ?
if 'a' contains '1' . -> 1 xor 1 (MASK is full of 1's) = 0.
if 'a' contains '0' . -> 0 xor 1(MASK is full of 1's) = 1.

So to summarise ,
1) First we complement only 32 actual bits.
2) Then we complement entire huge integer in python.
That means ,virtually the actual 32 bits remain same, but only the huge extra bits beyond 32nd bit are set to '1' , which initially were set to '0'.
Why did we do this ? To tell python that this is to be treated as negative integer , because it exceeded the max integer possible in 32 bit size.
Why should I explicitly tell? Because there is no size bound on integers , and it possibly fills with '0' which means mostly all 32 bit negatives might be treated as  positive integers.
Now whatever be the arbitrary size of that integer , because we tricked around to make leftmost bit to '1' . python looks at it as negative integer . 
Then the value of that negative integer is found using 2's complement method.
i.e start from right most bit, don't invert any bit until u come across first '1' . then invert all bits , to find the magnitude. then just put a negative sign infront of it.
