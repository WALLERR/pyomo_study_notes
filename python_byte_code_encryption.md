Our encryption method came from a link

https://my.oschina.net/u/4636319/blog/4960257

using byte code encryption method.



But unfortunately the site is lost. Related key words include 'byte code', 'encryption', 'python', 'pyc', etc. You can search other related posts online.



But I recall an approximate introduction of that.



The main topic about this post is to encrypt python code when compiling a .py file into a .pyc file. Usually a .pyc file can be easily decompiled by some methods into a .py file. In our application, we package our main.py into main.exe by using pyinstaller, which generates main.exe and related packages, as well as compiling all our .py files into .pyd files. Basically .pyd files cannot be decompiled. However, .exe files can be transformed into .pyc file and then decompiled into .py files easily. Hence more encryption method has to be implemented here.

When compiling .py files into .pyc files, compiler uses a dictionary to record each operation in Python like plus, minus with a number as below. The number is called **byte code**.

> #define POP_TOP                   199
> #define ROT_TWO                   253
> #define ROT_THREE                 232
> #define DUP_TOP                   4
> #define DUP_TOP_TWO               5
> #define NOP                       9
> #define UNARY_POSITIVE           10
> #define UNARY_NEGATIVE           11



By default, the order of these operations and their code is fixed. And compiled byte code is in numeric order, from 1 to 256 approximately.

When compiling .py files into .pyc files, these byte codes are used to transformed scripting languages into various numbers I guess, maybe somewhat machine languages. And when decompiling, the program transforms various numbers into script languages by these default byte codes. Byte codes are used as a dictionary.



Hence, if we revise the byte codes in our Python interpreter, then .pyc files cannot be decompiled into .py files using the default byte code dictionary normally. Even the transition is completed by some methods. The code is hard to read and understand.



There are two files recording this dictionary:

> your_environment/include/opcode.h
> 
> your_environment/Lib/opcode.py

You must revise the number **simultaneously**.



In opcode.py, I revised the first three number, for example. (From 1, 2, 3 to 199, 253, 232). The range of number may be [1, 255], which depends on the code internally.

```python
opname = ['<%r>' % (op,) for op in range(256)]

def_op('POP_TOP', 199)
def_op('ROT_TWO', 253)
def_op('ROT_THREE', 232)
def_op('DUP_TOP', 4)
def_op('DUP_TOP_TWO', 5)
```

Then in opcode.h, similarly. I made revision as below.

```
#define POP_TOP 199
#define ROT_TWO 253
#define ROT_THREE 232
#define DUP_TOP 4
#define DUP_TOP_TWO 5
```

After revising these two files, your compiling environment is prepared and if you try to compile .py files into .pyc files. Decompiling would be hard. You can change any number of byte code as you will but be sure to keep synchronicity.



Remember, the byte code does not depend on the virtual environment.

If you revise your byte code files in one virtual environment, two files in all other virtual environments will change as well, except your base environment. I do not know why but it does.

So if you want to restore your byte code files, just copy the files in base and replace yours in virtual environment with base files.



Other reference link:

[python opcode置换 - 简书](https://www.jianshu.com/p/f0c6130f7b13)
