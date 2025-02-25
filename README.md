# CS6235 A1 - Alias Analysis

    
## Instructions for Assignment 1:

Alias Analysis is given below.

https://www.cse.iitm.ac.in/~krishna/cs6235/a1.html

You need to implement the `internalTransform` method in `AliasAnalysis.java` class under the ***submit_a1*** package.

 `A1` is the main class. It takes the input file from ***inputs*** folder and queries file from ***queries*** folder.
 
 #### Running from command-line:
 `java -Dtest.file="queries/Q1.txt" A1 -pp -cp "inputs" -p jb use-original-names:true -app -src-prec java P1`
 
- The queries file path is given by -Dtest.file, which can be accessed using System.getProperty("test.file")
- **-pp** : indicates to prepend java class path to soot classpath. Initially soot classpath is empty
- **-cp "inputs"** : indicates to include "inputs"(where P1.java is placed) in soot classpath 
- **-p jb use-original-names:true** : in the jb pack (jimple body), use original names as of source file. This is required to retain the variable names, as our  queries use the java sorce file names . Otherwise, the jimple format will have different variable names.
- **-app** : indicates to run in application mode. Whatever classes referred from argument classes (here P1) will also be application classes(except library classes). Application classes are the ones that will be processed by soot.
- **-src-prec java** : indicates the source precision to be java file instead of .class file. This is to retain original names of variables. If source format is .class, the bytecode variable names will appear in jimple format.

All these options are encoded in the `getOptions` method of `A1` class.

If you want to give any further options or change the existing options, you can try them by passing command line arguments. For normal functioning required for assignment, you need not change any options. 
 
 
      
#### Query form:

Each query is of the form
*&lt;class&gt;:&lt;method&gt;:&lt;var1&gt; alias &lt;var2&gt;*
      
It represents, "Inside class *&lt;class&gt;*, method *&lt;method&gt;*, is *&lt;var1&gt;* alias of *&lt;var2&gt;* at the end of method *&lt;method&gt;*? "
      
Your analysis should answer either "Yes" or "No" for this.

The answers should follow the same order as of queries in the query file.
      
Your answers should be present in static String array `answers` in `A1` class which can be accessed as `A1.answers`.

#### Example:

Consider the below example program (in TACojava form),
      
```java
class P1 {
    public static void main(String[] args) {
    A x;
    A y;
    x = new A();
    y = new A();
    x.foo();
    y.foo(); 
  }
	
  public int m2() {
    A a;
    A b;
    A z;
    int ret;
    boolean c;
    ret = 0;
    c = true;
    a = new A();
    b = new A();
    if(c)
      z = a;
    else
      z = b;
    z.foo();
    return ret;
  } 
}

class A{
  public int foo() {
    int i;
    i = 10;
    System.out.println(i);
    return i;
  }
}
```
      
The Queries
      
```
P1:main:x alias y
P1:m2:a alias z
P1:m2:a alias b
P1:m2:b alias z
```

should answer
```      
No
Yes
No
Yes
```
It means, A1.answers[0] should contain No, A1.answers[1] should contain Yes, and so on..
