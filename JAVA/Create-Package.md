Create JAVA Package and Reuse Code
===

###Example:

**Create the DisplayName class that we want to reuse**
```java
package yiyu.codelab.java; // Need to create a package that means DisplayName.java belongs to 	yiyu.codelab.java
public class DisplayName {
public static String displayName(String first, String last) {
    return "<" + first + "," + last + ">";
  }
}
```
```
% javac DisplayName.java

Put the DisplayName.class file in the directory <class-path>/yiyu/codelab/java/
```


**Then if you want to use this DisplayName class, you need to import it.**
```java
import yiyu.codelab.java.DisplayName;
public class DisplayNameTest {
  public static void main (String args[]) {
    System.out.println(DisplayName.displayName("Yi", "Yu"));
  }
}
```
```
So, when the compiler see the class DisplayName, it will search all the paths in CLASSPATH and current directory.
That means, if you put the yiyu/codelab/java/ in current directory is OK.
if you put in /xxx/yyy/zzz/yiyu/codelab/java/, then you need to add /xxx/yyy/zzz to the environment variable CLASSPATH
```


**(Optional) Add /xxx/yyy/zzz to CLASSPATH in Mac**
```
% CLASSPATH=/xxx/yyy/zzz
% export CLASSPATH
```
