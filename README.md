---
layout: home
---

# Typelevel Scala

## What is this repository?

This [repository][tls] contains the [Typelevel][typelevel] [fork][fork] of the Scala compiler.

Typelevel Scala is a conservative, collaborative and binary compatible fork of [Lightbend Scala][lbs]. The intention
is for it to provide early access to bug fixes and enhancements of the Scala toolchain which are of particular
interest to [projects][projects] which use the "Typelevel style" &mdash; extensive use of type classes, generic
programming and exploitation of the distinctive features of Scala's type system such as higher-kinded, dependent and
singleton types.

## Relationship with Lightbend Scala

Typelevel Scala releases will be made in lockstep with Lightbend Scala and will by default generate code which is
binary compatible with code generated by Lightbend Scala. Code generated with Typelevel Scala can be freely linked
against binary dependencies generated with Lightbend Scala and vice versa. This allows Typelevel Scala features and
fixes to be used with minimal risk.

The policy for inclusion of a fix or a feature in a Typelevel Scala release is as follows,

+ It must be first submitted as a pull request to the corresponding version of Lightbend Scala.
+ It must have a reasonable likelihood of being merged in a future Lightbend Scala release.
+ It must offer a significant fix or feature for which there is no reasonable alternative or workaround.
+ It must maintain binary compatibility with Lightbend Scala unless a binary change is the primary motivation for the
  addition.

The aim of this policy is to keep Typelevel Scala as close as possible to Lightbend Scala whilst still providing
significant benefits to people who are affected by the issues it addresses. The requirement that all changes exist as
a pull request against Lightbend Scala _first_ and have a reasonable chance of being merged in a future Lightbend
Scala release is intended to encourage collaboration and convergence between Typelevel and Lightbend Scala
contributors.

Apart from the first criterion there is room for interpretation &mdash; after all, what counts as "reasonable"? Where
the letter of the policy is unclear we appeal to its spirit &mdash; we want access to these features and fixes in our
projects _now_ and for that to be feasible it is essential that we maintain the greatest possible degree of binary
interoperability with the rest of the Scala ecosystem.

## Typelevel Scala releases

Currently Typelevel Scala is available as a drop in replacement for Lightbend Scala 2.11.8. As soon as Lightbend Scala
2.12.0-RC1 is published a corresponding release of Typelevel Scala will be published.

### Typelevel Scala 2.11.8

The Typelevel Scala additions to Lightbend Scala 2.11.8 can be found on the branch
[2.11.8-bin-typelevel][2.11.8-bin-typelevel] of this repository.

Typelevel Scala 2.11.8 offers the following fixes and features over Lightbend Scala 2.11.8,

+ Support for partial unification (ie. a fix for [SI-2712][SI-2712]) &mdash; merged in Lightbend Scala 2.12.0-RC1.

  An improvement to type inference for type constructors, enabled by the `-Ypartial-unification` scalac option. This
  has many benefits for libraries, such as Cats and Scalaz, which make extensive use of higher-kinded types.

  Full details on the [pull request][pr-2712].
+ Support for literal types (aka SIP-23) &mdash; proposed for Lightbend Scala 2.12.1.

  Implements [literal types][SIP-23]. Enabled by `-Yliteral-types`.

  Literals can now appear in type position, designating the corresponding singleton type. A `scala.ValueOf[T]` type
  class and corresponding `scala.Predef.valueOf[T]` operator has been added yielding the unique value of types with a
  single inhabitant. Support for `scala.Symbol` literal types has been added.
+ A partial fix for SI-7046 &mdash; proposed for Lightbend Scala 2.12.1.

  The macro API call `knownDirectSubclasses` now yields the correct result in most cases and will report an error in
  cases where it is unable to yield the correct result.

  This is only a partial fix because subclasses defined in local scopes might be missed by `knownDirectSubclasses`. In
  mitigation it is very likely that a local subclass would represent an error in any scenario where
  `knownDirectSubclasses` might be used. An error will be reported in these cases.
  
  Full details on the [pull request][pr-7046].
+ A fix for SI-9760 &mdash; merged in Lightbend Scala 2.12.0-RC1.

  Higher kinded type arguments are now refined by GADT pattern matching. Details can be found on [the
  ticket][SI-9760].

## Should I use Typelevel Scala? In production?

The baseline for Typelevel Scala is the corresponding version of Lightbend Scala. Every bugfix or feature addition to
Typelevel Scala exists as a pull request against that Lightbend Scala version, so will have been passed by the full
Scala toolchain test suite and, in most cases, will have been reviewed by the Lightbend Scala compiler team.

If you are affected by one of the bugs which Typelevel Scala addresses then you will have to weigh the risks and costs
of using a compiler with the bug (perhaps with workarounds using unnecessarily complex encodings, macros or compiler
plugins), against the risks and costs of using an alternative Scala distribution which fixes that bug. Only you can
make that call. The same considerations apply to the additional features that Typelevel Scala supports.

More generally there are many reasons why you might want to use and contribute to Typelevel Scala,

+ You want to evaluate or contribute to a feature or fix.
+ You want to explore how new language features interact with existing libraries.
+ You want to explore new library designs enabled by new language features.
+ You want to propose new language features motiviated by your experiences with library design.
+ You want to contribute a feature or fix to Lightbend/Typelevel Scala and want to evaluate its interactions with
  other pending features and fixes.

Within the Typelevel family of projects we are particularly excited by the prospect of being able to coevolve
libraries along with the language and believe that this is one of the best ways to keep the language fresh and
relevant to practitioners.

## How to use Typelevel Scala

There are just two requirements for using Typelevel Scala in your existing projects,

+ You must be using (or be able to switch to) a corresponding version of Lightbend Scala. Currently this is 2.11.8
  with 2.12.0-RC1 expected as soon as it is released by Lightbend.
+ You must be using (or be able to switch to) SBT 0.13.13-RC2 or later. Earlier versions of SBT don't have full
  support for using an alternative `scalaOrganization`.

If you are using Lightbend Scala 2.11.8 and SBT 0.13.x the following steps will build your project with Typelevel
Scala,

+ Update your `project/build.properties` to require SBT 0.13.13-RC2,

  ```
  sbt.version=0.13.13-RC2
  ```

+ Add the following to your `build.sbt` immediately next to where you set `scalaVersion`,

  ```
  scalaOrganization := "org.typelevel"
  ```

  Alternatively, if you want to try Typelevel Scala without modifying your `build.sbt` you can instead create a file
  `local.sbt` at the root of your project with the following content,

  ```
  scalaOrganization in ThisBuild := "org.typelevel"
  ```

  This will be merged with your main build definitions and can be added to `.gitignore` or `.git/info/exclude` if so
  desired.

Now your build should function as before but using the Typelevel Scala toolchain instead of the Lightbend one. You can
verify this from the SBT prompt,

```
> show scalaOrganization
[info] org.typelevel
>
```

This will immediately provide you with the fixes for [SI-7046][SI-7046] and [SI-9760][SI-9760]. To additionally enable
the the fix for [SI-2712] and the implementation of [SIP-23] you should add either or both of their enabling flags to
`scalacOptions` (or `local.sbt` if that was the path you took earlier),

```
scalacOptions += "-Ypartial-unification" // enable fix for SI-2712
scalacOptions += "-Yliteral-types"       // enable SIP-23 implementation

```

Note that Typelevel Scala 2.11.8 replaces the [si2712fix compiler plugin][si2712fix-plugin] &mdash; if you are using
it you should remove it from your build before switching to Typelevel Scala.

Also note that the two compiler flags above should be used in preference to `-Xexperimental` at present &mdash; as
well as enabling the above two features `-Xexperimental` also enables some other features which are not typically
desirable.

You now can verify that these features have been enabled from the SBT console,

```
> console
[info] Compiling 3 Scala sources to /home/miles/projects/value-wrapper/target/scala-2.11/classes...
[info] Starting scala interpreter...
[info] 
Welcome to Scala 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_102).
Type in expressions for evaluation. Or try :help.

scala> val foo: "foo" = "foo" // Use of literal type "foo"
foo: "foo" = foo

scala> import scala.language.higherKinds
import scala.language.higherKinds

scala> def foo[F[_], A](fa: F[A]): String = fa.toString
foo: [F[_], A](fa: F[A])String

scala> foo((x: Int) => x*2)   // Function1[Int, Int] unifies with F[_]
res1: String = <function1>
```

## Roadmap

The following are high priority issues for Typelevel projects on which progress is likely to be made in 2016,

+ Partial type application.
+ Multiple implicit parameter blocks.
+ Improved compile times for inductive implicits.
+ Improved control over implicit prioritization.
+ Improved error reporting for implicit resolution failures.
+ GADT and singleton type bugfixes.
+ Literal syntax for Byte and Short types.
+ Support for alternative `scala.Predef`.

In accordance with the policy for inclusion contributions on these issues will be made as pull requests against
Lightbend Scala in the first instance.

## How to contribute to Typelevel Scala

Because of our policy of "pull request against Lightbend Scala first" the primary route to contributing to Typelevel
Scala is by contributing to Lightbend Scala. The Lightbend Scala team have made huge advances in lowering the barrier
to participation in compiler development recently, in large part due to a new SBT-based build, and [helpful
documentation][lbs-readme] &mdash; the Scala distribution is now an SBT project which you can expect to work on in
much the same way as any other github hosted FLOSS Scala project. [Miles Sabin][milessabin] has provided a write up
(now slightly outdated) of the process [here][scalac-post].

Because a pull request against [scala/scala][lbs] is the first step, to contribute you will need to sign the [Scala
CLA][scala-cla] and be willing to make your contributions under the [Scala License][scala-license].  Typelevel is
strongly in favour of the currently Scala license/CLA (which requires contributors to grant a patent license but does
not grant one in return) being replaced by the Apache 2.0 [license][apache-2.0-license] and [ICLA][apache-2.0-icla]
(which both requires and grants a patent license) as soon as is feasible. If you would like to contribute but are
unable to sign the CLA please raise this issue on the [Lightbend][lbs-gitter] and [Typelevel][tls-gitter] gitter
channels.

## Community

Topics specific to Typelevel Scala are discussed on its [gitter channel][tls-gitter]. Scala compiler development is
discussed on the [scala/contributors gitter channel][lbs-gitter].

People are expected to follow the [Typelevel Code of Conduct][coc] when discussing Typelevel Scala on the Github page,
Gitter channel, or other venues.

We hope that our community will be respectful, helpful, and kind. If you find yourself embroiled in a situation that
becomes heated, or that fails to live up to our expectations, you should disengage and contact one of the [project
maintainers](#maintainers) in private. We hope to avoid letting minor aggressions and misunderstandings escalate into
larger problems.

If you are being harassed, please contact one of [us](#maintainers) immediately so that we can support you.

## Maintainers

The current maintainers (people who can merge pull requests) are:

 + Erik Osheim ([@non](https://github.com/non))
 + George Leontiev ([@folone](https://github.com/folone))
 + Jon Pretty ([@propensive](https://github.com/propensive))
 + Lars Hupel ([@larsrh](https://github.com/larsrh))
 + Mike O'Connor ([@stew](https://github.com/stew))
 + Miles Sabin ([@milessabin](https://github.com/milessabin))
 + Tom Switzer ([@tixxit](https://github.com/tixxit))

[tls]: https://github.com/typelevel/scala
[fork]: http://typelevel.org/blog/2014/09/02/typelevel-scala.html
[lbs]: https://github.com/scala/scala
[projects]: http://typelevel.org/projects/
[SI-2712]: https://issues.scala-lang.org/browse/SI-2712
[pr-2712]: https://github.com/scala/scala/pull/5102
[SI-7046]: https://issues.scala-lang.org/browse/SI-7046
[pr-7046]: https://github.com/scala/scala/pull/5284
[SI-9760]: https://issues.scala-lang.org/browse/SI-9760
[SIP-23]: https://github.com/scala/scala/pull/5310
[si2712fix-plugin]: https://github.com/milessabin/si2712fix-plugin
[tls-gitter]: https://gitter.im/typelevel/scala
[lbs-gitter]: https://gitter.im/scala/contributors
[typelevel]: http://typelevel.org/
[coc]: http://typelevel.org/conduct.html
[lbs-readme]: https://github.com/scala/scala/blob/2.12.x/README.md
[scalac-post]: http://milessabin.com/blog/2016/05/13/scalac-hacking/
[milessabin]: https://github.com/milessabin
[2.11.8-bin-typelevel]: https://github.com/typelevel/scala/commits/2.11.8-bin-typelevel
[scala-cla]: http://www.lightbend.com/contribute/cla/scala
[scala-license]: https://github.com/scala/scala/blob/2.12.x/doc/LICENSE.md
[apache-2.0-license]: http://www.apache.org/licenses/LICENSE-2.0
[apache-2.0-icla]: https://www.apache.org/licenses/icla.txt
