# Robocode-OutputBot

You may find the following information useful. These are links to Sun's online Java tutorials.

- [Overview of I/O Streams](https://docs.oracle.com/javase/tutorial/essential/io/streams.html)
- [How to use PrintStream](http://java2s.com/Tutorials/Java/Stream_Reader_Writer/How_to_use_Java_PrintStream.htm)

# Specifications

Write (or modify an existing) bot that prints information about the robot it's tracking to a file. It should also output predictive targeting information to help you debug it.

## Detail

You will probably want to modify an existing bot that you've written to date, rather than start over from scratch. The notes below make this assumption.

1. Add these import statements to to the top of your robot

	```java
	import java.io.*;
	import java.text.NumberFormat;
	```
2. Create two new private class variables inside your robot: a [PrintStream](https://docs.oracle.com/javase/7/docs/api/java/io/PrintStream.html) (call it ps) and a [NumberFormat](https://docs.oracle.com/javase/7/docs/api/java/text/NumberFormat.html) (call it f).
3. Near the top of your run() method, before the while (true) loop, set up the `NumberFormat` object like so:
    1. Construct it, not with a constructor, but by calling the static `getNumberInstance` method like so:
        
	```java
        f = NumberFormat.getNumberInstance();
        ```
	
    2. Set the maximum number of decimal places you want to have shown by calling the `setMaximumFractionDigits()` method. 	(Hint: pass it a small number like, oh say, 1 or 2.)
4. Just after you set up the NumberFormat class, set up the output file like so:
	1. Get a reference to the data file you can use by calling `getDataFile()` like so:
		```
		File file = getDataFile("output.dat");
		```
	2. Construct a new `RobocodeFileOutputStream`, passing it the reference to the data file you just got, and pass the new `RobocodeFileOutputStream` to the constructor for a new `PrintStream`. (Look [here](https://docs.oracle.com/javase/tutorial/essential/io/datastreams.html) for an example of nested constructors and see if you can translate it over to what you need for this step.) Store the new `PrintStream` in the 'ps' class variable you declared earlier.
	
	Note #1: Do not re-declare a local 'ps' var inside `run()`, it will create oodles of problems.

	Note #2: You will need to handle the exception that could be thrown when constructing the new `PrintStream` by using a try..catch statement. An example of try..catch statement using `PrintStream` can be found [here](http://java2s.com/Tutorials/Java/Stream_Reader_Writer/How_to_use_Java_PrintStream.htm)

5. Down in your `doGun()` method (or wherever you're doing your targeting) use the `PrintStream` object to print output to your file. Here's an example to get you started:
	```java
	ps.println("Tracking: " + enemy.getName() + " at (x,y) (" + 
		f.format(enemy.getX()) + ", " + f.format(enemy.getY()) + ")");
	```
	
6. You will need to close your output stream when the round is over. There are two ways the round can end for you: your bot dies, or your bot wins. To handle both of these, please ovverride both the onWin and onDeath methods and put the following code in there:
	```java
	if (ps != null) ps.close();
	```

## Sample Output

Here's an example of the kind of output I'm looking for. You might output a little bit more or a little bit less, but you should end up with something like this:

```
Tracking: sample.Fire at (x,y) (384.1, 53.2)
  firePower = 2.1, bulletSpeed = 13.7, time = 13
  futureX = 384.1, futureY = 53.2, absDeg = 97.3, turn = 2.4

Tracking: sample.Fire at (x,y) (384.1, 53.2)
  firePower = 2.1, bulletSpeed = 13.7, time = 13
  futureX = 384.1, futureY = 53.2, absDeg = 99.7, turn = 2.4

Tracking: sample.Fire at (x,y) (384.1, 53.2)
  firePower = 2.1, bulletSpeed = 13.7, time = 13
  futureX = 384.1, futureY = 53.2, absDeg = 102.1, turn = 2.4

Tracking: sample.RamFire at (x,y) (181.4, 197)
  firePower = 3, bulletSpeed = 11, time = 9
  futureX = 181.4, futureY = 197, absDeg = 344.2, turn = -117.9

Tracking: sample.RamFire at (x,y) (181.4, 197)
  firePower = 3, bulletSpeed = 11, time = 9
  futureX = 181.4, futureY = 197, absDeg = 341, turn = -97.1

```
# Submission 

Upload your robot that has OutputBot functionality into this repository
