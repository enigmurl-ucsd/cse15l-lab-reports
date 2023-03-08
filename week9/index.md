# CSE 15L Lab Report 5 | CSE 15L Redo
## Author: Manu Bhat - A17337644 - mbhat@ucsd.edu

### CSE 15L Redo 

For this lab report, I will be redoing the bug exploration we did in week 3 using jdb, and explain in detail the steps I went through.

# Array Methods 

To compile the code with debug symbols, I used the `-g` flag in javac.

```javac -g -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java```

Then to debug, I used this command:

```jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests```

To find the error, I set a breakpoint on the methods of interest. Here is my debugger output to realize that the bug was eventually that the modifying in place overwrites itself. Here is the output! I made use of the locals and print command, as well as the stop at.

```
(base) manubhat@manus-laptop lab9 % jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
Initializing jdb ...
> stop at ArrayTests.testReverseInPlace
Deferring breakpoint ArrayTests.testReverseInPlace.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore ArrayTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
>
VM Started: JUnit version 4.13.2
Set deferred breakpoint ArrayTests.testReverseInPlace
.
Breakpoint hit: "thread=main", ArrayTests.testReverseInPlace(), line=7 bci=0
7        int[] input1 = { 3, 4 };

main[1] locals
No local variables
main[1] step
>
Step completed: "thread=main", ArrayTests.testReverseInPlace(), line=8 bci=8
8        ArrayExamples.reverseInPlace(input1);

main[1] locals
Method arguments:
Local variables:
input1 = instance of int[2] (id=984)
main[1] print input1[0]
 input1[0] = 3
main[1]
```

# List Methods 

I used the same debugging classes, so I only had to do a different run, which uses the following command:

```jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListTests```

To debug this, I set a breakpoint on each of the tests methods, in addition, I used the step command. I did use the same testing file as I had in lab3, but I made sure to do the actual debugging commands myself (so the testers really only server as a view of the bugs, rather than providing the reasoning). Here is the output, for where I realized that only `index` was getting updated in the while loop.

```
jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListTests
Initializing jdb ...
> ls
Unrecognized command: 'ls'.  Try help...
> stop at ListTests.testMerge
Deferring breakpoint ListTests.testMerge.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore ListTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
>
VM Started: JUnit version 4.13.2
Set deferred breakpoint ListTests.testMerge
..
Breakpoint hit: "thread=Time-limited test", ListTests.testMerge(), line=23 bci=0
23            ArrayList<String> x = new ArrayList<>();

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=24 bci=8
24            ArrayList<String> y = new ArrayList<>();

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=25 bci=16
25            y.add("Good1");

Time-limited test[1] list
21        @Test(timeout = 10000)
22        public void testMerge() {
23            ArrayList<String> x = new ArrayList<>();
24            ArrayList<String> y = new ArrayList<>();
25 =>         y.add("Good1");
26            assertEquals(y, ListExamples.merge(x,y));
27        }
28
29        @Test(timeout = 10000)
30    public void testMergeGood() {
Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=26 bci=23
26            assertEquals(y, ListExamples.merge(x,y));

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=25 bci=0
25        List<String> result = new ArrayList<>();

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=26 bci=8
26        int index1 = 0, index2 = 0;

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=27 bci=13
27        while(index1 < list1.size() && index2 < list2.size()) {

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=37 bci=108
37        while(index1 < list1.size()) {

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
> E
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
> .
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 1
index2 = 0
Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 2
index2 = 0
Time-limited test[1] step
>

Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] stpe
Unrecognized command: 'stpe'.  Try help...
Time-limited test[1] step
>
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 3
index2 = 0
Time-limited test[1]
```

# Linked List Methods 

I used the same debugging classes, so I only had to do a different run, which uses the following command:

```jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore LinkedListTests```

I used a similar technique to the above two, and those few instructions pretty much allow me to do anything! One thing I did use differently was step up, which was necessary since I accidentallyed used step instead of next in one place. I was able to realize that the list method append was not acting properly, which led to the size of the linkedlist acting funnily. Another thing that made testing hard was that the timeout limit was being reached while I did debugging, so JUnit marked the test wrong before I evn finished, which I resolved by using a higher threshold so I could actually debug. Heres my full output:

```
(base) manubhat@manus-laptop lab9 % jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListTests
Initializing jdb ...
> ls
Unrecognized command: 'ls'.  Try help...
> stop at ListTests.testMerge
Deferring breakpoint ListTests.testMerge.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore ListTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
> 
VM Started: JUnit version 4.13.2
Set deferred breakpoint ListTests.testMerge
..
Breakpoint hit: "thread=Time-limited test", ListTests.testMerge(), line=23 bci=0
23            ArrayList<String> x = new ArrayList<>();

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=24 bci=8
24            ArrayList<String> y = new ArrayList<>();

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=25 bci=16
25            y.add("Good1");

Time-limited test[1] list
21        @Test(timeout = 10000)
22        public void testMerge() {
23            ArrayList<String> x = new ArrayList<>();
24            ArrayList<String> y = new ArrayList<>();
25 =>         y.add("Good1");
26            assertEquals(y, ListExamples.merge(x,y));
27        }
28    
29        @Test(timeout = 10000)
30    public void testMergeGood() {
Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListTests.testMerge(), line=26 bci=23
26            assertEquals(y, ListExamples.merge(x,y));

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=25 bci=0
25        List<String> result = new ArrayList<>();

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=26 bci=8
26        int index1 = 0, index2 = 0;

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=27 bci=13
27        while(index1 < list1.size() && index2 < list2.size()) {

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=37 bci=108
37        while(index1 < list1.size()) {

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
> E
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
> .
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 1
index2 = 0
Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 2
index2 = 0
Time-limited test[1] step
> 

Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=41 bci=141
41        while(index2 < list2.size()) {

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=42 bci=152
42          result.add(list2.get(index2));

Time-limited test[1] stpe
Unrecognized command: 'stpe'.  Try help...
Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", ListExamples.merge(), line=43 bci=170
43          index1 += 1;

Time-limited test[1] locals
Method arguments:
list1 = instance of java.util.ArrayList(id=1009)
list2 = instance of java.util.ArrayList(id=1010)
Local variables:
result = instance of java.util.ArrayList(id=1011)
index1 = 3
index2 = 0
Time-limited test[1] cont
> Time: 78.197
There was 1 failure:
1) testMerge(ListTests)
org.junit.runners.model.TestTimedOutException: test timed out after 10000 milliseconds
	at app//ListTests.testMerge(ListTests.java:26)

FAILURES!!!
Tests run: 3,  Failures: 1


The application exited
(base) manubhat@manus-laptop lab9 % jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore LinkedListTests
Initializing jdb ...
> stop at LinkedListTests.testBigAppend
Deferring breakpoint LinkedListTests.testBigAppend.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore LinkedListTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
> 
VM Started: JUnit version 4.13.2
Set deferred breakpoint LinkedListTests.testBigAppend
..
Breakpoint hit: "thread=Time-limited test", LinkedListTests.testBigAppend(), line=15 bci=0
15            LinkedList a = new LinkedList();

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", LinkedList.<init>(), line=14 bci=0
14        public LinkedList() {

Time-limited test[1] step up
> 
Step completed: "thread=Time-limited test", LinkedListTests.testBigAppend(), line=15 bci=7
15            LinkedList a = new LinkedList();

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedListTests.testBigAppend(), line=16 bci=8
16            a.append(0);

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedListTests.testBigAppend(), line=17 bci=13
17            a.append(1);

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedListTests.testBigAppend(), line=18 bci=18
18            a.append(2);

Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=30 bci=0
30            if(this.root == null) {

Time-limited test[1] list
26         * Adds the value to the _end_ of the list
27         * @param value
28         */
29        public void append(int value) {
30 =>         if(this.root == null) {
31                this.root = new Node(value, null);
32                return;
33            }
34            // If it's just one element, add if after that one
35            Node n = this.root;
Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=35 bci=21
35            Node n = this.root;

Time-limited test[1] next
> E
Step completed: "thread=Time-limited test", LinkedList.append(), line=36 bci=26
36            if(n.next == null) {

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=41 bci=47
41            while(n.next != null) {

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=42 bci=54
42                n = n.next;

Time-limited test[1] next
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=41 bci=47
41            while(n.next != null) {

Time-limited test[1] list
37                n.next = new Node(value, null);
38                return;
39            }
40            // Otherwise, loop until the end and add at the end with a null
41 =>         while(n.next != null) {
42                n = n.next;
43                n.next = new Node(value, null);
44            }
45        }
46    
Time-limited test[1] step
> 
Step completed: "thread=Time-limited test", LinkedList.append(), line=44 bci=62
44            }

Time-limited test[1] print this

Time: 167.634
There was 1 failure:
1) testBigAppend(LinkedListTests)
org.junit.runners.model.TestTimedOutException: test timed out after 10000 milliseconds
	at app//LinkedListTests.testBigAppend(LinkedListTests.java:16)

FAILURES!!!
Tests run: 2,  Failures: 1

 this = "0 1 "
Time-limited test[1] cont
> 
The application exited
```

# File Methods 

Again, more or less same running command.

```
jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore FileTests      
```

This one was relatively hard to debug in that there was a lot of recursion to keep track of, however I was able to figure it out in the end. Essentially, the problem is that it adds non leaf nodes when it shouldnt, and does not actually complete the recursive step. This was pretty obvious after I printed out the contents of the list after it added the current directory.

```
(base) manubhat@manus-laptop lab9 % jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore FileTests
Initializing jdb ...
> stop at FileExample.getFiles
Deferring breakpoint FileExample.getFiles.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore FileTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
>
VM Started: JUnit version 4.13.2
.Set deferred breakpoint FileExample.getFiles

Breakpoint hit: "thread=main", FileExample.getFiles(), line=42 bci=0
42    		File f = start;

main[1] cont
> .
Breakpoint hit: "thread=main", FileExample.getFiles(), line=42 bci=0
42    		File f = start;

main[1] cont
> E
Time: 3.354
There was 1 failure:
1) testDirectory(FileTests)
java.lang.AssertionError: expected:<[some-files/a.txt, some-files/even-more-files/a.txt, some-files/even-more-files/d.java, some-files/more-files/b.txt, some-files/more-files/c.java]> but was:<[some-files, some-files/.DS_Store, some-files/even-more-files, some-files/more-files, some-files/a.txt]>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at FileTests.testDirectory(FileTests.java:9)

FAILURES!!!
Tests run: 2,  Failures: 1


The application exited
(base) manubhat@manus-laptop lab9 % jdb -classpath .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore FileTests
Initializing jdb ...
> stop at FileExample.getFiles
Deferring breakpoint FileExample.getFiles.
It will be set after the class is loaded.
> run
run org.junit.runner.JUnitCore FileTests
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
>
VM Started: JUnit version 4.13.2
.Set deferred breakpoint FileExample.getFiles

Breakpoint hit: "thread=main", FileExample.getFiles(), line=42 bci=0
42    		File f = start;

main[1] next
>
Step completed: "thread=main", FileExample.getFiles(), line=43 bci=2
43    	  List<File> result = new ArrayList<>();

main[1] next
>
Step completed: "thread=main", FileExample.getFiles(), line=44 bci=10
44    	  result.add(start);

main[1] print result
 result = "[]"
main[1] next
>
Step completed: "thread=main", FileExample.getFiles(), line=45 bci=18
45    	  if(f.isDirectory()) {

main[1] locals
Method arguments:
start = instance of java.io.File(id=987)
Local variables:
f = instance of java.io.File(id=987)
result = instance of java.util.ArrayList(id=985)
main[1] print result
 result = "[some-files/a.txt]"
main[1] exit
(base) manubhat@manus-laptop lab9 %
```
