## Instructions of running
- run using ``python3 parser.py Inputfile OutputFile SymbolTableFile``

## SymbolTable : 
- This is a list of symbol Tables; One for global and one each for functions and classes
- each local and temp varible contain base address, offset and size
- All entries for activation record are in function entry of symbol table


## 3AC :
### BeginFunc 
```
  push %rbp
  mov %rsp %rbp
  sub <local_var_space> %rsp
  push <callee saved registers>
```

### EndFunc
```
  pop <callee saved registers>
  mov %rbp %rsp
  pop %rbp
  ret
```
### Some Specific Points:
  - We currently generate 2 3AC code file(code.crux, code2.crux), one contains names of variable, while other contain address, relative to stack or heap
  - load_address a , b ::  a = &b :: put address of b in variable a
  - load a, b :: a = *b;
  - eq a, b :: *a = b
  - eqconst a, b :: a = b

## Features :
- if,if-else
- break,continue
- for,while,do-while
- Functions, Function Overloading
- Classes
- new , delete
- Vanilla C features

Following are the differences from vanilla implementation of the above[^1]
## Miscelaneous
- Variable sized arrays cannot be declared :
  ```
  int a[5]; //allowed
  int a[c]; //not allowed
  ```
## Branches
 - Same as vanilla C++ except following changes
 - We have no else if clause so you have to write if-else as :
   ```
   if(i==0){
      y=1;
   }else{
      if(i==1){
        y=2;
      }
   }
   ```
 - You have to use curly braces for all loops,branches,etc... single lines not allowed
   ```
   if(i==0) y=1;    //not allowed
   if(i==0){ y=1; } //allowed
   ```
## Loops
- All 3 for, while and do-while loops are allowed and same as vanilla C++ except for the compulsory use of braces
  ```
  for(;;)int i=0; //not allowed
  for(;;){;}      //allowed
  ```
- break gets attached to nearest switch-case/\{for,while,do-while\}loop, while continue gets attached to nearest \{for,while,do-while\}loop. See tests/34.cpp.

## Classes
- Class variables are accessed only by ``this``.
- Class function can be prototyped inside class declaration itself
- Class function should be defined outside of class as in given example, it does not needs to prototyped for definition.
- No Constructor and Destructor, we have to call them excplicitly.
- Class functions can be prototyped inside class and can be added later using "::" operator.
  ```
  class x{
    int a;
    int f(int b);
  };
  class x:: int f(int y){
    	if(this->a > y){
       	return 0;
      }else{
      	return 1;
      }
  }
  ```

## Functions
- Function overloading allowed.
  - 2 Functions are same only if thier name are same, and order of type of arguments are exactly same.
  - Class Functions can also be overloaded.
  - return type overloading is not allowed. E.g.
    ```
    void func(int b, int c){;}  //1,2,3,4 are all different
    void func(int b, float c){;}
    void func(int b){;}
    void func1(int b, int c){;}
    int func(int a, int h){return 1;} //same as 1
    ```
- Function code is generated as follows:
  - offset of all local variables except parameters are calculated during function definition.
  - offset of parameters is calculated during function call.
  - Stack space is increased using offset calculated earlier using BeginFunc and EndFunc reverts to original space

## Include
- Presently you can only include std.cpp, it contains function for new and delete. And include statement(if used) should be at the top of the file.

## Allocation/Deallocation
- new keyword is used for allocation and is used as ```new (int)[c]``` round brackets are compulsory and square braces are optional and if mentioned returns array of allocated objects. Here variable sized arrays are possible.
- to delete a variable previously allocated, use ``delete id`` and for array use ``delete [] id``. Currently the id should be an identifier only.

[^1]: Reference for 3AC : [Stanford 3AC Examples](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/handouts/240%20TAC%20Examples.pdf )


