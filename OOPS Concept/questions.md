
1. Can we create a program without main method?

Prior to Java 7: Yes, sequence was as follows:
- jvm loads class
- executes static blocks
- looks for main method and invokes it

Before Java 7 we could write full code under static block and it used to run normally. The static block is first executed as soon as the class is loaded before the main() method is invoked. Then main() is also declared as static; therefore, Java doesn't need an object to call the main() method.

Let's take an example

// This program successfully runs prior to JDK 7 
public class Test  
{ 
    // static block 
    static
    { 
        System.out.println("Hello User"); 
    } 
}
If you execute above piece of code then compiler presumes Test is that class which main() is defined in. Therefore, JVM loads the class and static block gets executed. So in above example, JVM runs static block first and then comes to know that there is no main() method in the class. Therefore, it throws "exception", as exception comes while execution. If we don't want an exception to be thrown, we can terminate the program by System.exit(0);

But from JDK7 main method is mandatory. Compiler will verify first, whether main() is present or not. If your program doesn't have a main method, you will get an error main method not found in the class. This error is generated during byte code verification because in byte code, main is not there.


2. Why is multiple inheritance not supported in Java?

Java doesn’t support multiple inheritances in classes because it can lead to diamond problem and rather than providing some complex way to solve it, there are better ways through which we can achieve the same result as multiple inheritances.

Diamond Problem in Java
To understand diamond problem easily, let’s assume that multiple inheritances were supported in java. In that case, we could have a class hierarchy like below image.diamond problem in java
https://journaldev.nyc3.cdn.digitaloceanspaces.com/2013/07/diamond-problem-multiple-inheritance-450x264.png

Let’s say SuperClass is an abstract class declaring some method and ClassA, ClassB are concrete classes. SuperClass.java

```
package com.journaldev.inheritance;

public abstract class SuperClass {

	public abstract void doSomething();
}
```

```
package com.journaldev.inheritance;

public class ClassA extends SuperClass{
	
	@Override
	public void doSomething(){
		System.out.println("doSomething implementation of A");
	}
	
	//ClassA own method
	public void methodA(){
		
	}
}
```

```
package com.journaldev.inheritance;

public class ClassB extends SuperClass{

	@Override
	public void doSomething(){
		System.out.println("doSomething implementation of B");
	}
	
	//ClassB specific method
	public void methodB(){
		
	}
}
```
```
package com.journaldev.inheritance;

/* this is just an assumption to explain the diamond problem
this code won't compile*/
public class ClassC extends ClassA, ClassB{

	public void test(){
		//calling super class method
		doSomething();
	}

}
```

Notice that test() method is making a call to superclass doSomething() method. This leads to the ambiguity as the compiler doesn’t know which superclass method to execute. Because of the diamond-shaped class diagram, it’s referred to as Diamond Problem in java. The diamond problem in Java is the main reason java doesn’t support multiple inheritances in classes. Notice that the above problem with multiple class inheritance can also come with only three classes where all of them has at least one common method.

Multiple Inheritance in Java Interfaces


You might have noticed that I am always saying that multiple inheritances is not supported in classes but it’s supported in interfaces. A single interface can extend multiple interfaces, below is a simple example. InterfaceA.java

```
package com.journaldev.inheritance;

public interface InterfaceA {

	public void doSomething();
}
```

```
package com.journaldev.inheritance;

public interface InterfaceB {

	public void doSomething();
}
```

```
package com.journaldev.inheritance;

public interface InterfaceC extends InterfaceA, InterfaceB {

	//same method is declared in InterfaceA and InterfaceB both
	public void doSomething();
	
}
```
This is perfectly fine because the interfaces are only declaring the methods and the actual implementation will be done by concrete classes implementing the interfaces. So there is no possibility of any kind of ambiguity in multiple inheritances in Java interfaces. That’s why a java class can implement multiple interfaces, something like below example. InterfacesImpl.java

```
package com.journaldev.inheritance;

public class InterfacesImpl implements InterfaceA, InterfaceB, InterfaceC {

	@Override
	public void doSomething() {
		System.out.println("doSomething implementation of concrete class");
	}

	public static void main(String[] args) {
		InterfaceA objA = new InterfacesImpl();
		InterfaceB objB = new InterfacesImpl();
		InterfaceC objC = new InterfacesImpl();
		
		//all the method calls below are going to same concrete implementation
		objA.doSomething();
		objB.doSomething();
		objC.doSomething();
	}

}

```



