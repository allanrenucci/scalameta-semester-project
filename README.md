# Scalameta Semester Project
Documentation/Website: https://scalameta.org/

Repository: https://github.com/scalameta/scalameta

## Getting Started with Scalameta
```
$ git clone https://github.com/scalameta/scalameta.git
```

The project is built using [`sbt`](https://www.scala-sbt.org/).
```
$ sbt
> scalametaJVM/compile
> testsJVM/test
```

### Writting and running your first test
Create a new `.scala` file in `tests/shared/src/test/scala/scala/meta/tests/`. Here is a template
you can copy-paste:
```scala
package org.scalameta.tests

import org.scalatest._

import scala.meta._

class MyTest extends FunSuite {

  test("my first test") {
    val program = "class Foo"
    println(program.parse[Source].get)
  }
}
```

You can then run your test with `sbt`:
```scala
> testsJVM/testOnly org.scalameta.tests.MyTest
```

### Experimenting with the `console`
```
> scalametaJVM/console
[info] Starting scala interpreter...
[info] 
Welcome to Scala 2.12.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_172).
Type in expressions for evaluation. Or try :help.

scala> import scala.meta._
import scala.meta._
```

### Parsing, AST, Tokens...
```scala
scala> val program = "class Foo"
program: String = class Foo

scala> val tree = program.parse[Source].get
tree: scala.meta.Source = class Foo

scala> val tokens = tree.tokens
tokens: scala.meta.tokens.Tokens = Tokens(, class,  , Foo, )
```

Sources:
- [Parser](https://github.com/scalameta/scalameta/blob/master/scalameta/parsers/shared/src/main/scala/scala/meta/internal/parsers/ScalametaParser.scala)
- [Tokens](https://github.com/scalameta/scalameta/blob/master/scalameta/tokens/shared/src/main/scala/scala/meta/tokens/Token.scala)
- [Trees](https://github.com/scalameta/scalameta/blob/master/scalameta/trees/shared/src/main/scala/scala/meta/Trees.scala)


## Getting Started with Dotty
Documentation/Website: http://dotty.epfl.ch/

Repository: https://github.com/lampepfl/dotty

Getting started with `sbt`:
```
$ sbt new lampepfl/dotty.g8
$ sbt
sbt:dotty-example-project> console
scala> enum Foo { case Bar }
// defined class Foo
```

### Dotty Enums
Useful links:
- Documentation:
  - [Enumerations](http://dotty.epfl.ch/docs/reference/enums/enums.html)
  - [Algebraic Data Types](http://dotty.epfl.ch/docs/reference/enums/adts.html)
- Pull Requests:
  - [Initial implementation](https://github.com/lampepfl/dotty/pull/1958)
  - [Simplification](https://github.com/lampepfl/dotty/pull/4003)

## Support for Dotty Enums in Scalameta
Open issue: https://github.com/scalameta/scalameta/issues/901

### TODO list:

**Week 1:**
- [x] Be familiar with Scalameta and its concepts (e.g. Trees, Tokens, Parser).
      Read the [Tree Guide](https://scalameta.org/docs/trees/guide.html).
- [x] Be familiar with the Scalameta codebase (e.g. `sbt`, compile, test).
- [x] Be familiar with Dotty Enums. In particular their syntax.
- [x] Think about a tree representation for enums. Some thoughts are already in
      the [Scalameta issue](https://github.com/scalameta/scalameta/issues/901).
      However, this was before the [simplification](https://github.com/lampepfl/dotty/pull/4003).
      
**Week 2:**
- [ ] Implement and test the parser for the "Case".
 
 ### DONE:
 
**Week 1:**
1. Read scalameta files that are interesting for the project
2. Using AST explorer and code, understand how the AST is built
3. Read the TreeGuide
3. Study the Dotty syntax for Enum to compare with Class syntax
4. Have a first idea of what need to be changed : 
  * In the method that begins at the line 2872 of scalametaParser: 
  ```scala
  def tmplDef(mods: List[Mod]): Member with Stat
  ```
    adding a case for the token *KwEnum*, with a structure like the *KwClass* case
  * In Trees.scala
  ```scala
  @ast class Enum(mods: List[Mod],
                     name: scala.meta.Type.Name,
                     tparams: List[scala.meta.Type.Param],
                     ctor: Ctor.Primary,
                     templ: Template) extends Defn with Member.Type
  ```
  
  for the Enum Case  : 
  
  ```scala
  @ast class Case(mods : List[Mod],
                    name : scala.meta.Type.Name,
                    tparams : List[scala.meta.Type.Param],
                    ctor : Option[Ctor.Primary) extends Defn with Member.Type]
  ```
