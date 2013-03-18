LArray
=== 
A library for managing large arrays that can hold more than 2G (2^31) entries in Java and Scala.

## Features 
 * 2^31 (2GB) is the limitation of the default Java/Scala array size, because 32-bit signed integer (int) is used for the array indexes. To resolve this, LArray uses long type indexes that uses 64-bit signed integer.
   * For example the entire human genome data (3GB) can be stored in LArray. 
 * LArray can be released from the main memory at any time. 
   * The default arrays in Java/Scala consume JVM heaps heavily and often cause OutOfMemory exception when working with such large amount of data. 
   * Call LArray.free to release acquired memory resources immediately.
   * Even if you forget to call LArray.free, the acquired memory will be released when GC sweeps LArray instances.
 * LArray is free from the limitation of JVM memory manager.
   * LArray uses memory space outside of the default JVM heap, so creating LArrays with more than -Xmx(maximum memory size) is possible. This is useful when you need large amount of memory or its size is unknown.


## Supported Platforms

LArray uses OS-specific implementation for copying memory contents between LArray and Java arrays. Currently, the following CPU architecutres are supported:

 * Windows (32/64-bit)
 * Linux (amd64 (Intel 64-bit), arm, armhf)
 * Mac OSX (Intel 64bit)

In addition, Oracle JVM (standard JVM) must be used since LArray depends on `sun.misc.Unsafe` class.

## Usage (Scala)
Add the following sbt dependencies to your project settings:

```scala
libraryDependencies += "org.xerial" % "larray" % "0.1"
```

### When using snapshot version
```scala
resolvers += "Sonatype shapshot repo" at "https://oss.sonatype.org/content/repositories/snapshots/"

libraryDependencies += "org.xerial" % "larray" % "0.1-SNAPSHOT"
```

## Usage (Scala)

You can use LArray in the same manner with the standard Scala Arrays: 

```scala
import xerial.larray.LArray

val l = LArray(1, 2, 3)
val e = l(0) // 1
println(l.mkString(", ")) // 1, 2, 3
l(1) = 5
println(l.mkString(", ")) // 1, 5, 3
    
val l2 = LArray[Int].ofDim(10000L)
```

## Usage (Java)

In Java we cannot provide concise syntaxes as in Scala. Instead, use `apply` and `update` methods to read/write values in arrays.

```java
import xerial.larray.LIntArray;

LIntArray l = new LIntArray(10000L);
l.update(0L, 20) // Set l[0L] = 20
l.apply(0L)  //  Get l[0L]
```
