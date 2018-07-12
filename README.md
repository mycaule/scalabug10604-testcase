### Scala language issue 1064
https://github.com/scala/bug/issues/10604

Let's write a [very big `case class`](VeryBigRecord.scala) with more then 100 fields.
The use case was for a machine learning task.

```
case class VeryBigRecord(
    x001: Int,
    x002: Int,
    x003: Int,
    x004: Int,
    x005: Int,
    x006: Int,
    ...
    x178: Int
)
```

This fails at compilation.
```
$ scalac VeryBigRecord.scala
error: java.lang.StackOverflowError
	at scala.tools.nsc.typechecker.Typers$Typer.checkDead(Typers.scala:111)
...
```

When increasing Java heap size it compiles.
```
$ JAVA_OPTS="-Xss2048K" scalac VeryBigRecord.scala
```

See [scalac-output](scalac-output) for the full error.

My machine was running a default setup of Scala:
```
$ scala -version
Scala code runner version 2.11.8 -- Copyright 2002-2016, LAMP/EPFL

$ uname -a
Linux ams 4.9.0-6-amd64 #1 SMP Debian 4.9.88-1+deb9u1 (2018-05-07) x86_64 GNU/Linux
```
