## Version 1.0.0-MF

> 2017 Aug 3

`MF` stands for milestone final. This is the last non-RC release before 1.0.0.
 The main purpose/focus of this release is to offer a relatively stable API to
 work with prior to 1.0.0. It can be deemed as a proposal for the final API
 we are going to maintain binary compatibility after 1.0.
 We will give community some time to validate it before we release 1.0.0-RC1.
 
### To migrate from 0.9.0

We apologize for the number of breaking changes in this release. We are trying to include
as many breaking changes as possible in this release before we lock down the API. 
  
 * `cats` no longer publishes the all-inclusive bundle package `"org.typelevel" % "cats"`, use `cats-core`, `cats-free`, or `cats-law` 
   accordingly instead. If you need `cats.free`, use `"org.typelevel" % "cats-free"`, if you need `cats-laws` use 
   `"org.typelevel" % "cats-laws"`, if neither, use `"org.typelevel" % "cats-core"`.
 * `cats.free.Inject` is moved from `cats-free` to `cats-core` and renamed to `cats.InjectK`;
   `cats.data.Prod` is renamed to `cats.data.Tuple2K`; `cats.data.Coproduct` is renamed to
   `cats.data.EitherK`
 * All `Unapply` enabled methods, e.g. `sequenceU`, `traverseU`, etc. are removed. `Unapply`
   enabled syntax ops are also removed. Please use the partial unification SI-2712 fix
   instead. The easiest way might be this [sbt-plugin](https://github.com/fiadliel/sbt-partial-unification).
 *  `FunctorFilter`, `MonadCombine`, `MonadFilter`, `MonadReader`, `MonadState`, `MonadTrans`, `MonadWriter` and `TraverseFilter` are no longer in `cats`, the functionalities they provided are inherited by the new [cats-mtl](https://github.com/edmundnoble/cats-mtl) project. Please check [here](https://github.com/edmundnoble/cats-mtl#migration-guide) for migration guide.
 * `CartesianBuilder` (i.e. `|@|`) syntax is deprecated, use the apply syntax on tuples instead. E.g. `(x |@| y |@| z).map(...)` should be replaced by `(x, y, z).mapN(...)`. If you are getting "`mapN` not found" error message, it could be due to SI-2712, see the 3rd migration item above. 
 * Apply syntax on tuple (e.g. `(x, y, z).map3(...)`)  was moved from `cats.syntax.tuple._` to `cats.syntax.apply._` and renamed to `mapN`, `contramapN` and `imapN` respectively.
 * The creation methods (`left`, `right`, `apply`, `pure`, etc.) in `EitherT` were improved to take less
   type arguments.
 * Several `cats-core` type class instances for `cats.kernel` were moved from their companion objects to separate traits
   and thus require imports from `cats.instances.xxx._` (or the recommended `import cats.implicits._`) now. See #1659 for more details. 
 * `Free.suspend` is renamed to `Free.defer` for consistency. 
 * `traverse1_`, `intercalate1` and `sequence1_` in `Reducible` were renamed to `nonEmptyTraverse_`, `nonEmptyIntercalate` and `nonEmptySequence_` respectively. 
 * `foldLeftM` is removed from `Free`, use `foldM` on `Foldable` instead, see #1117 for detail. 
 * `iteratorFoldM` was removed from `Foldable` due to #1716
 * `Split` is removed, and the method `split` is moved to `Arrow`. Note that only under `CommutativeArrow` does it guarantee the non-interference between the effects. see #1567

If you feel adventurous you can try the experimental Scalafix rewrites.
See all the available rewrites and the instructions [here](/scalafix/README.md).
### Breaking Changes:

 * [#1614](https://github.com/typelevel/cats/pull/1614): added `leftT` and improved existing lift API for `EitherT`. by @kailuowang
 * [#1596](https://github.com/typelevel/cats/pull/1596): Rename `Inject` to `InjectK`. by @andyscott
 * [#1589](https://github.com/typelevel/cats/pull/1589): Rename `Prod`, `Coproduct` to `Tuple2K` and `EitherK`. by @kailuowang
 * [#1583](https://github.com/typelevel/cats/pull/1583): Enable SI-2712 fix in cats / Remove unapply machinery. by @kailuowang
 * [#1679](https://github.com/typelevel/cats/pull/1679): remove `Unapply` class. by @kailuowang
 * [#1557](https://github.com/typelevel/cats/pull/1557): Improvements to `Inject`. @sellout
 * [#1659](https://github.com/typelevel/cats/pull/1659): move instances into separate trait. by @yilinwei
 * [#1709](https://github.com/typelevel/cats/pull/1709): Rename `suspend` to `defer`. by @peterneyens
 * [#1611](https://github.com/typelevel/cats/pull/1611): Renamed `traverse1_`, `intercalate1` and `sequence1_` in `Reducible`. by @LukaJCB
 * [#1117](https://github.com/typelevel/cats/pull/1117): `foldLeftM` without `Free`. by @TomasMikula 
 * [#1487](https://github.com/typelevel/cats/pull/1487): `Apply` syntax for tuples. by @DavidGregory084
 * [#1745](https://github.com/typelevel/cats/pull/1745): Deprecate `CartesianBuilder`. by @kailuowang
 * [#1758](https://github.com/typelevel/cats/pull/1758): stop publishing cats all bundle , start to publish cats-testkit. by @kailuowang
 * [#1766](https://github.com/typelevel/cats/pull/1766): Replace `Split` with `CommutativeArrow`, introduces `CommutativeMonad`. by @diesalbla
 * [#1751](https://github.com/typelevel/cats/pull/1751): Removed `FunctorFilter`, `MonadCombine`, `MonadFilter`, `MonadReader`, `MonadState`, `MonadTrans`, `MonadWriter`, `TraverseFilter`. by @edmundnoble
 
### New Features (API, instances, data types, etc):

 * [#1707](https://github.com/typelevel/cats/pull/1707): Add NEL/NEV one. by @peterneyens 
 * [#1680](https://github.com/typelevel/cats/pull/1680): ~~`MonadTrans` instance for RWST and make `MonadTrans` serializable.~~ by @wedens
 * [#1658](https://github.com/typelevel/cats/pull/1658): Add `Validated.validNel`. by @edmundnoble
 * [#1651](https://github.com/typelevel/cats/pull/1651): ~~Add state method to `MonadState`.~~ by @oskoi
 * [#1628](https://github.com/typelevel/cats/pull/1628): add init and size methods to `NonEmptyList`. by @jtjeferreira
 * [#1612](https://github.com/typelevel/cats/pull/1612): Add ensureWith to `Validated` and `Either` (#1550). by @LukaJCB
 * [#1598](https://github.com/typelevel/cats/pull/1598): Implement a `ReaderWriterStateT` data type . by @iravid
 * [#1706](https://github.com/typelevel/cats/pull/1706): Clean up `ReaderWriterStateT`. by @peterneyens
 * [#1594](https://github.com/typelevel/cats/pull/1594): Add `NonEmptyList#fromFoldable`. by @markus1189
 * [#1611](https://github.com/typelevel/cats/pull/1611): Added `NonEmptyTraverse`. by @LukaJCB
 * [#1592](https://github.com/typelevel/cats/pull/1592): added instances of `BitSet` to `allInstances`. by @kailuowang
 * [#1586](https://github.com/typelevel/cats/pull/1586): Add `Applicative.unit`. by @alexandru
 * [#1584](https://github.com/typelevel/cats/pull/1584): Move arbitrary instance of `StateT` to laws. by @kailuowang
 * [#1580](https://github.com/typelevel/cats/pull/1580): add `groupBy` to `NonEmptyList` and `groupByNel` to `List` syntax @julien-truffaut
 * [#1578](https://github.com/typelevel/cats/pull/1578): add `last`, `sortBy` and `sorted` to `NonEmptyList`. by @julien-truffaut
 * [#1571](https://github.com/typelevel/cats/pull/1571): added `whileM`, `untilM`, `iterateWhile`, etc to `Monad` . by @tpolecat & @kailuowang
 * [#1548](https://github.com/typelevel/cats/pull/1548): `MonadError` instance for `Ior`. by @leandrob13
 * [#1543](https://github.com/typelevel/cats/pull/1543): `MonadError` instance for `Kleisli`. by @durban
 * [#1540](https://github.com/typelevel/cats/pull/1540): `Ior` syntax. by @leandrob13
 * [#1537](https://github.com/typelevel/cats/pull/1537): Add `FlatMap.forEffect`. by @cranst0n
 * [#1531](https://github.com/typelevel/cats/pull/1531): Add piecemeal import for `MonadError`. by @peterneyens
 * [#1526](https://github.com/typelevel/cats/pull/1526): `Inject` for free programs. by @tpolecat
 * [#1464](https://github.com/typelevel/cats/pull/1464): Adding `get` for `Foldable`. by @yilinwei
 * [#1602](https://github.com/typelevel/cats/pull/1602): Stack-safe `Coyoneda`. by @edmundnoble
 * [#1725](https://github.com/typelevel/cats/pull/1725): Add `InjectK` laws. by @andyscott
 * [#1728](https://github.com/typelevel/cats/pull/1728): Adds an `As` class which represents subtyping relationships (`Liskov`). by @stew
 * [#1178](https://github.com/typelevel/cats/pull/1178): Add `Is` constructor for Leibniz equality. by @tel
 * [#1611](https://github.com/typelevel/cats/pull/1611): Add `NonEmptyTraverse` typeclass. by @LukaJCB
 * [#1736](https://github.com/typelevel/cats/pull/1736): Added `StackSafeMonad` mixin. by @djspiewak
 * [#1600](https://github.com/typelevel/cats/pull/1600): `Inject` for `Either`. by @andyscott
 * [#1746](https://github.com/typelevel/cats/pull/1746): Add `EitherNel` type alias for `Either[NonEmptyList[E], A]`. by @andyscott
 * [#1670](https://github.com/typelevel/cats/pull/1670): Add `Order`-> `Ordering` implicit conversion to implicits, instances. by @edmundnoble
 * [#1649](https://github.com/typelevel/cats/pull/1649): Make `Show` inherit from a contravariant base trait for `show` string interpolator to be covariant. by @edmundnoble
  * [#1761](https://github.com/typelevel/cats/pull/1761): Add index related helpers to `Traverse`. by @andyscott 
  * [#1769](https://github.com/typelevel/cats/pull/1769): Add `Kleisli` `tap`, `tapWith`. by @tpolecat
  * [#1739](https://github.com/typelevel/cats/pull/1739): Add `onError` and `adaptError` to `ApplicativeError`/`MonadError`. by @SystemFw
  * [#1644](https://github.com/typelevel/cats/pull/1644): Add `MonadError` instance for `EitherT` that recovers from `F[_]` errors. by @leandrob13 
  * [#1748](https://github.com/typelevel/cats/pull/1748): Stack-safe `FreeAppplicative`. by @edmundnoble
  * [#1516](https://github.com/typelevel/cats/pull/1516): Implement `NonEmptyList#Collect` . by @xavier-fernandez
 
  
### Code improvements:

 * [#1660](https://github.com/typelevel/cats/pull/1660): Override `fromTry` and `fromEither` for `Try` and `Either`. by @peterneyens
 * [#1642](https://github.com/typelevel/cats/pull/1642): Unseal `InjectK` to allow for extension by other libraries. by @andyscott
 * [#1641](https://github.com/typelevel/cats/pull/1641): Make `InjectK` use `FunctionK.id` for reflexive injection. by @andyscott
 * [#1618](https://github.com/typelevel/cats/pull/1618): Override some methods in `Kleisli` instances. by @peterneyens
 * [#1532](https://github.com/typelevel/cats/pull/1532): Override `Foldable` methods. by @peterneyens
 * [#1456](https://github.com/typelevel/cats/pull/1456): Consistency for ops classes. by @edmundnoble
 * [#1631](https://github.com/typelevel/cats/pull/1631): make all `PartialApplied` class value class to achieve zero cost. by @kailuowang
 * [#1696](https://github.com/typelevel/cats/pull/1696): Make `syntax.show` extend `ShowSyntax` instead of `Show.ToShowOps`. by @edmundnoble
 
### Bug fixes: 

* [#1735](https://github.com/typelevel/cats/pull/1735): `StateT` no longer violates laws. by @djspiewak
* [#1740](https://github.com/typelevel/cats/pull/1740):  removed `iteratorFoldM`. by @kailuowang

### Other miscellaneous improvements (documentation, tests, build):

 * [#1699](https://github.com/typelevel/cats/pull/1699): Link to sbt-partial-unification plugin . by @Blaisorblade   
 * [#1698](https://github.com/typelevel/cats/pull/1698): Update gitter chat room name to cats-dev. . by @kailuowang 
 * [#1695](https://github.com/typelevel/cats/pull/1695): update ETA for 1.0.0 . by @kailuowang   
 * [#1604](https://github.com/typelevel/cats/pull/1604): Add tut doc for `FunctionK` . by @ceedubs
 * [#1691](https://github.com/typelevel/cats/pull/1691): Build JVM before JS on travis. by @peterneyens
 * [#1677](https://github.com/typelevel/cats/pull/1677): Update readme with the new dev channel.. by @kailuowang
 * [#1673](https://github.com/typelevel/cats/pull/1673): Use 2 workers in JVM build. by @ceedubs
 * [#1671](https://github.com/typelevel/cats/pull/1671): Fixing `Eq[Function1]` in testsJS; break JS build to separate matrix build. by @kailuowang
 * [#1666](https://github.com/typelevel/cats/pull/1666): Use `Cogen` for arbitrary instances. by @ceedubs
 * [#1654](https://github.com/typelevel/cats/pull/1654): Update Circe URL. by @n4to4
 * [#1653](https://github.com/typelevel/cats/pull/1653): Fix typo in `FreeApplicative` doc.. by @takayuky
 * [#1647](https://github.com/typelevel/cats/pull/1647): Adds Freestyle to `Related Projects` list. by @raulraja
 * [#1638](https://github.com/typelevel/cats/pull/1638): Make simulacrum a compile time only dependency. by @peterneyens
 * [#1637](https://github.com/typelevel/cats/pull/1637): show(f:T) to show(t:T). by @PeterPerhac
 * [#1636](https://github.com/typelevel/cats/pull/1636): added some category theory into `FunctionK` document. by @kailuowang
 * [#1632](https://github.com/typelevel/cats/pull/1632): upgraded to scala 2.12.2 and 2.11.11 and scalaJs. by @kailuowang
 * [#1629](https://github.com/typelevel/cats/pull/1629): add unit test for variance on methods in `EitherT`. by @jtjeferreira
 * [#1622](https://github.com/typelevel/cats/pull/1622): Update `Discipline` and `ScalaTest`. by @peterneyens
 * [#1615](https://github.com/typelevel/cats/pull/1615): Fix doc for `InvariantMonoidal`. by @BenFradet
 * [#1609](https://github.com/typelevel/cats/pull/1609): Include `Id` docs in the menu. by @ceedubs
 * [#1591](https://github.com/typelevel/cats/pull/1591): Improve test coverage. by @peterneyens
 * [#1590](https://github.com/typelevel/cats/pull/1590): Check monad laws for `Cokleisli`. by @peterneyens
 * [#1588](https://github.com/typelevel/cats/pull/1588): Docs/Tutorial -- Simplify `Kleisli` example. by @RawToast
 * [#1581](https://github.com/typelevel/cats/pull/1581): restore the alphabetical order of maintainers list. by @kailuowang
 * [#1575](https://github.com/typelevel/cats/pull/1575): minor improvements to `tailRecM` doc. by @kailuowang
 * [#1570](https://github.com/typelevel/cats/pull/1570): fixed a paragraph order. by @kailuowang
 * [#1566](https://github.com/typelevel/cats/pull/1566): Fix mistake in documentation of `Group.remove`. by @LukaJCB
 * [#1563](https://github.com/typelevel/cats/pull/1563): Remove references of the NEL `OneAnd` alias. by @peterneyens
 * [#1561](https://github.com/typelevel/cats/pull/1561): Fix incorrect numbering in `FreeMonads` doc. by @cb372
 * [#1555](https://github.com/typelevel/cats/pull/1555): fix scala.js badge version. by @xuwei-k
 * [#1551](https://github.com/typelevel/cats/pull/1551): added `MonadError` and `ApplicativeError` to hierarchy diagram. by @kailuowang
 * [#1547](https://github.com/typelevel/cats/pull/1547): fix ref to non-existent dir in contributing. by @sullivan-
 * [#1546](https://github.com/typelevel/cats/pull/1546): add to `Monad` `ifM` example. by @sullivan-
 * [#1545](https://github.com/typelevel/cats/pull/1545): fix scaladoc for `Eval` methods `Unit`, `True`, `False`, `Zero`, `One`. by @sullivan-
 * [#1541](https://github.com/typelevel/cats/pull/1541): Switch from CrossVersion.full to CrossVersion.patch for TLS compatibi…. by @milessabin
 * [#1530](https://github.com/typelevel/cats/pull/1530): add a favicon for sbt-microsite. by @larsrh
 * [#1529](https://github.com/typelevel/cats/pull/1529): Fix typo in `Applicative` doc.. by @cranst0n
 * [#1525](https://github.com/typelevel/cats/pull/1525): Remove link to apply.html from menu. by @Leammas
 * [#1693](https://github.com/typelevel/cats/pull/1693): Clean up EitherT doctests. by @peterneyens
 * [#1697](https://github.com/typelevel/cats/pull/1697): Added two links to the learner page. by @kailuowang
 * [#1726](https://github.com/typelevel/cats/pull/1726): Add underscore.io Advanced Scala with Cats. by @DieBauer
 * [#1718](https://github.com/typelevel/cats/pull/1718): Fixed some things in the build. by @djspiewak
 * [#1734](https://github.com/typelevel/cats/pull/1734): update sbt. by @jyane
 * [#1737](https://github.com/typelevel/cats/pull/1737): Rewrote documentation on the IO monad to reference cats-effect. by @djspiewak
 * [#1744](https://github.com/typelevel/cats/pull/1744): Make links link to the `.html` files instead of `.md`. by @LukaJCB
 * [#1759](https://github.com/typelevel/cats/pull/1759): Faster tests by reducing the size of lists. @peterneyens
 * [#1760](https://github.com/typelevel/cats/pull/1760): Decrease stack-safety test size. by @edmundnoble
 * [#1752](https://github.com/typelevel/cats/pull/1752): More coverage. by @edmundnoble
 * [#1472](https://github.com/typelevel/cats/pull/1472): Using regular syntax in the FreeApplicative tutorial. by @denisftw
 * [#1565](https://github.com/typelevel/cats/pull/1565): added instance table to docs, enhanced typeclass diagram. by @kailuowang
 * [#1573](https://github.com/typelevel/cats/pull/1573): Add symbols to FAQ. by @zainab-ali 
 
 
## Version 0.9.0

> 2017 January 15

The biggest user-facing change in this release is to the behavior of the `flatMap` (and related methods) provided by `EitherOps` for the standard library's `Either` for Scala 2.10 and 2.11. These methods now match the behavior of the `flatMap` on `Either` in Scala 2.12 in that they don't require the left-hand side types to match.

For example, the following would previously compile on 2.12, but not 2.10 or 2.11:

```scala
import cats.syntax.either._

sealed abstract class AppError
case object Error1 extends AppError
case object Error2 extends AppError

val either1: Either[Error1.type, String] = Right("hi")
val either2: Either[Error2.type, String] = Right("bye")

val result: Either[AppError, String] = for {
  v1 <- either1
  v2 <- either2
} yield v1 + v2
```

This code now works equivalently on all supported Scala versions.

Changes:

 * [#1424](https://github.com/typelevel/cats/pull/1424): `NonEmptyList` and `NonEmptyVector` are now covariant
 * [#1506](https://github.com/typelevel/cats/pull/1506): `flatMap` provided by `Either` syntax matches 2.12's `Either#flatMap`
 * [#1466](https://github.com/typelevel/cats/pull/1466): Improved stack safety for `StateT`
 * [#1510](https://github.com/typelevel/cats/pull/1510): `catchNonFatal` for `Future` is now asynchronous

Bug fixes:

 * [#1465](https://github.com/typelevel/cats/pull/1465) and [#1507](https://github.com/typelevel/cats/pull/1507): Stack safety law for `Monad#tailRecM` is less eager and doesn't throw exceptions

New type class instances:

 * [#1475](https://github.com/typelevel/cats/pull/1475): `Reducible` instances for `Eval` and `Id`
 * [#1484](https://github.com/typelevel/cats/pull/1484): `Show` instance for `Symbol`

Other additions:

 * [#1446](https://github.com/typelevel/cats/pull/1446): `Cofree` comonad
 * [#1520](https://github.com/typelevel/cats/pull/1520) and [#1522](https://github.com/typelevel/cats/pull/1522): `intercalate` for `Foldable` (and `intercalate1` for `Reducible`)
 * [#1454](https://github.com/typelevel/cats/pull/1454): `asLeft` and `asRight` syntax methods for creating `Either` values
 * [#1468](https://github.com/typelevel/cats/pull/1468): `tupleLeft` and `tupleRight` for `Functor`
 * [#1500](https://github.com/typelevel/cats/pull/1500): `putLeft`, `putRight`, `mergeLeft`, and `mergeRight` methods for `Ior`
 * [#1495](https://github.com/typelevel/cats/pull/1495): `show` string interpolator
 * [#1448](https://github.com/typelevel/cats/pull/1448): `Validated#findValid` (like `orElse` but error accumulating)
 * [#1455](https://github.com/typelevel/cats/pull/1455): `reverse` for `NonEmptyList`
 * [#1517](https://github.com/typelevel/cats/pull/1517): `zipWithIndex` for `NonEmptyList`
 * [#1512](https://github.com/typelevel/cats/pull/1512) and [#1514](https://github.com/typelevel/cats/pull/1514): `filterNot` for `NonEmptyList` and `NonEmptyVector`
 * [#1480](https://github.com/typelevel/cats/pull/1480): `FunctionK#and`
 * [#1481](https://github.com/typelevel/cats/pull/1481): `EitherT.cond`

Miscellaneous improvements (syntax, documentation, tests):

 * [#1513](https://github.com/typelevel/cats/pull/1513): Improved documentation for `Functor`, `Applicative`, and `Traverse`
 * [#1440](https://github.com/typelevel/cats/pull/1440): Improved type class documentation
 * [#1442](https://github.com/typelevel/cats/pull/1442): Improved documentation for `Semigroup` and `Monoid`
 * [#1479](https://github.com/typelevel/cats/pull/1479): Some instance traits are now package-private
 * [#1445](https://github.com/typelevel/cats/pull/1445): Workaround for Tut issue
 * [#1477](https://github.com/typelevel/cats/pull/1477): Use new kind-projector syntax for polymorphic lambdas
 * [#1483](https://github.com/typelevel/cats/pull/1483): Binary compatibility checking is now part of the build for cats-kernel
 * [#1469](https://github.com/typelevel/cats/pull/1469): More consistent instance names
 * [#1496](https://github.com/typelevel/cats/pull/1496): Simpler creation of some `SemigroupK` and `MonoidK` instances
 * [#1490](https://github.com/typelevel/cats/pull/1490): Avoid some duplication in build via sbt-travisci
 * [#1497](https://github.com/typelevel/cats/pull/1497): Site list clean-up

And version updates:

 * [#1499](https://github.com/typelevel/cats/pull/1499): 2.12 version is now 2.12.1
 * [#1509](https://github.com/typelevel/cats/pull/1509): Scala.js version is 0.6.14

As always thanks to everyone who filed issues, participated in the Cats Gitter
channel, submitted code, or helped review pull requests.

## Version 0.8.1

> 2016 November 9

Version 0.8.1 is a release to support Scala 2.12.0 with no changes to published code (only tests and documentation).

Build:

* [#1457](https://github.com/typelevel/cats/pull/1457): Update to Scala 2.12.0

Miscellaneous improvements (syntax, documentation, tests):

* [#1444](https://github.com/typelevel/cats/pull/1444): Remove `defaultTailRecM` from monad doc
* [#1441](https://github.com/typelevel/cats/pull/1441): Fixes #1438 by replacing quoted entry name
* [#1432](https://github.com/typelevel/cats/pull/1432): Type class organization in documentation
* [#1439](https://github.com/typelevel/cats/pull/1439): Update version on index
* [#1451](https://github.com/typelevel/cats/pull/1451): Fix `Arbitrary` instances for ScalaCheck 1.13.3+

## Version 0.8.0

> 2016 October 25

Version 0.8.0 is the eighth Cats release, and the first release with support for Scala 2.12 (specifically the 2.12.0-RC2 release candidate).

Apart from the introduction of Scala 2.12 support, the biggest change in this release is the removal
of `Xor` and `XorT`. See the [FAQ](http://typelevel.org/cats/faq.html#either) for information about
the motivations for this change and recommendations for migration.

Removals and deprecations:

 * [#1310](https://github.com/typelevel/cats/pull/1310): `Xor` and `XorT` are gone
 * [#1370](https://github.com/typelevel/cats/pull/1370): `RecursiveTailRecM` and `Free#foldMapUnsafe` are gone and stack safety is checked in the laws for `Monad`
 * [#1411](https://github.com/typelevel/cats/pull/1411): `FreeT#interpret` is deprecated in favor of the (equivalent) `compile`

Additions:

 * [#1382](https://github.com/typelevel/cats/pull/1382), [#1415](https://github.com/typelevel/cats/pull/1415): Support for Scala 2.12.0-RC2
 * [#1414](https://github.com/typelevel/cats/pull/1414): `Foldable#iteratorFoldM` and lazy `foldM` implementations for many standard library instances
 * [#1356](https://github.com/typelevel/cats/pull/1356): `append` and `prepend` (and operator aliases) for `NonEmptyVector`
 * [#1327](https://github.com/typelevel/cats/pull/1327): `EitherT.fromOption`
 * [#1388](https://github.com/typelevel/cats/pull/1388): `StateT.set` and `StateT.setF`
 * [#1392](https://github.com/typelevel/cats/pull/1392): `StateT.get`
 * [#1325](https://github.com/typelevel/cats/pull/1325): `WriterT.lift`
 * [#1391](https://github.com/typelevel/cats/pull/1391): `MonadReader#reader`
 * [#1352](https://github.com/typelevel/cats/pull/1352): Macro-powered `FunctionK.lift`
 * [#1398](https://github.com/typelevel/cats/pull/1398): `<<<` and `>>>` aliases for `Compose`'s `compose` and `andThen`
 * [#1408](https://github.com/typelevel/cats/pull/1408): `toNestedValidated` and `toNestedValidatedNel` for `EitherT`
 * [#1399](https://github.com/typelevel/cats/pull/1399): `Order.fromComparable`
 * [#1394](https://github.com/typelevel/cats/pull/1394): `Traverse#flatSequence`
 * [#1417](https://github.com/typelevel/cats/pull/1417): `MonadTests#stackUnsafeMonad` laws for instances where `tailRecM` is known to be unsafe
 * [#1411](https://github.com/typelevel/cats/pull/1411): `compile` and `foldMap` for the `Free` and `FreeT` companion objects

New instances:

 * [#1319](https://github.com/typelevel/cats/pull/1319): `Order` and `Group` for `BigDecimal`
 * [#1354](https://github.com/typelevel/cats/pull/1354): `Semigroup` for `Ior`
 * [#1395](https://github.com/typelevel/cats/pull/1395): `Order` for `Symbol`
 * [#1324](https://github.com/typelevel/cats/pull/1324): `PartialOrder` and other instances for `BitSet`
 * [#1324](https://github.com/typelevel/cats/pull/1324): `Eq` and `PartialOrder` for `Either`
 * [#1324](https://github.com/typelevel/cats/pull/1324): `PartialOrder`, `Monoid`, and other instances for `Function0`
 * [#1324](https://github.com/typelevel/cats/pull/1324): `Monoid` and other instances for `Function1`
 * [#1402](https://github.com/typelevel/cats/pull/1402): `Monad`, `MonadCombine`, `Traverse`, `Order`, etc. for `Prod`
 * [#1413](https://github.com/typelevel/cats/pull/1413): `MonadError` for `StateT`
 * [#1399](https://github.com/typelevel/cats/pull/1399): Instances for `java.util.UUID`

Renaming and rearrangements:

 * [#1385](https://github.com/typelevel/cats/pull/1385): The `cats.js.std` package is now `cats.js.instances`
 * [#1324](https://github.com/typelevel/cats/pull/1324): Many instances moved from cats-core to cats-kernel
 * [#1394](https://github.com/typelevel/cats/pull/1394): `Traverse#traverseM` is now `flatTraverse`

Miscellaneous improvements (syntax, documentation, tests):

 * [#1347](https://github.com/typelevel/cats/pull/1347): Consistency laws for `combineAll` and `combineAllOption`
 * [#1324](https://github.com/typelevel/cats/pull/1324): Performance improvements for `Either` instances
 * [#1386](https://github.com/typelevel/cats/pull/1386): `FunctionK` tests and examples now use kind-projector 0.9's polymorphic lambdas
 * [#1410](https://github.com/typelevel/cats/pull/1410): Replace `Coproduct#run` with `Coproduct#fold`
 * [#1331](https://github.com/typelevel/cats/pull/1331): Less expensive `tailRecM`-`flatMap` consistency checking
 * [#1330](https://github.com/typelevel/cats/pull/1330): More consistent parameter-less method definitions and usage, other syntactic improvements
 * [#1340](https://github.com/typelevel/cats/pull/1340): New [Scaladex](https://index.scala-lang.org) badge
 * [#1416](https://github.com/typelevel/cats/pull/1416): New diagram of type classes
 * [#1352](https://github.com/typelevel/cats/pull/1352): API docs for `FunctionK`
 * [#1369](https://github.com/typelevel/cats/pull/1369), [#1418](https://github.com/typelevel/cats/pull/1418): New project site based on [sbt-microsites](https://github.com/47deg/sbt-microsites)
 * [#1259](https://github.com/typelevel/cats/pull/1259): 0.6-to-0.7 migration guide
 * [#1304](https://github.com/typelevel/cats/pull/1304), [#1317](https://github.com/typelevel/cats/pull/1317), [#1323](https://github.com/typelevel/cats/pull/1323), [#1350](https://github.com/typelevel/cats/pull/1350), [#1366](https://github.com/typelevel/cats/pull/1366), [#1376](https://github.com/typelevel/cats/pull/1376), [#1380](https://github.com/typelevel/cats/pull/1380), [#1390](https://github.com/typelevel/cats/pull/1390), [#1403](https://github.com/typelevel/cats/pull/1403), [#1407](https://github.com/typelevel/cats/pull/1407), [#1421](https://github.com/typelevel/cats/pull/1421): Other miscellaneous documentation improvements

Build:

 * [#1345](https://github.com/typelevel/cats/pull/1345): Update ScalaCheck (to 1.13.2) and Discipline (to 0.6)
 * [#1353](https://github.com/typelevel/cats/pull/1353): Generated sources are included in source jars
 * [#1322](https://github.com/typelevel/cats/pull/1322): Scala.js test clean-up
 * [#1426](https://github.com/typelevel/cats/pull/1426): Human-friendly names in metadata for published artifacts
 * [#1389](https://github.com/typelevel/cats/pull/1389): More memory for Travis CI

## Version 0.7.2

> 2016 September 1

Version 0.7.2 is a patch release that was released to fix a major bug
([#1346](https://github.com/typelevel/cats/issues/1346)) that appeared
in 0.7.0. It also contains several other improvements.

It should be safe to upgrade from 0.7.0 to 0.7.2 -- there are no major
API changes between these releases.

### Changes

Fixes:

 * [#1347](https://github.com/typelevel/cats/pull/1347): fixes broken `Monoid[Map[K, V]].combineAll` implementation.
 * [#1304](https://github.com/typelevel/cats/pull/1304): fix `CoflatMap` documentation.
 * [#1322](https://github.com/typelevel/cats/pull/1322): fix SBT commands (`release`, `validate`, etc.).
 * [#1311](https://github.com/typelevel/cats/pull/1311): rename some implicit instances for consistency.

Additions:

 * [#1319](https://github.com/typelevel/cats/pull/1347): add missing `BigDecimal` instances.
 * [#1324](https://github.com/typelevel/cats/pull/1324): add missing function and `BitSet` instances.

Note that 0.7.2 was preceeded by a botched 0.7.1. release. Please
avoid using this version of Cats -- it has major incompatibilities
with 0.7.0 and is not documented here.

## Version 0.7.0

> 2016 August 21

Version 0.7.0 is the seventh Cats release, and includes several major rearrangements and changes to names.

### Migration notes

If you're updating from Cats 0.6.0, it's likely that you'll need to make extensive (but mostly mechanical) changes. The following list includes some of the changes that are likely to be necessary for most projects; see the complete list of changes below for more detail.

* All references to `cats.std` will need to be changed to `cats.instances` ([#1140](https://github.com/typelevel/cats/pull/1140)). If you're using `cats.std.all` or the other `cats.std` objects with wildcard imports, this is likely to be the only change you need to make. If you are importing or referring to instance definitions by name, you'll need to be aware that the naming convention has changed (see [#1066](https://github.com/typelevel/cats/pull/1066), [#1068](https://github.com/typelevel/cats/pull/1068), [#1110](https://github.com/typelevel/cats/pull/1110), and [#1122](https://github.com/typelevel/cats/pull/1122)).
* `NonEmptyList` and `NonEmptyVector` are no longer type aliases for `OneAnd`, so any code using `OneAnd` to construct or pattern match on these types will need to be changed to use `NonEmptyList` or `NonEmptyVector` directly. There are also some API changes; for example, `unwrap` calls will need to be replaced by `toList` or `toVector`, and `NonEmptyList(1, 2, 3)` is now `NonEmptyList.of(1, 2, 3)`.
* `pureEval` has been removed from `Applicative` ([#1234](https://github.com/typelevel/cats/pull/1234)), and has not been replaced, so if you are relying on it for laziness or effect capturing (which wasn't enforced or guaranteed), you'll need to find another approach.
* All references to `NaturalTransformation` will need to be replaced by either `FunctionK` or `~>`.
* The `FlatMap` type class now has a `tailRecM` method that is designed to support stack-safe recursive monadic binding. If your monad's `flatMap` is stack safe, you can implement a stack-safe `tailRecM` by calling `Monad#defaultTailRecM`. The stack safety of `tailRecM` is not enforced, but if your implementation is stack safe, you should also provide an instance of the `RecursiveTailRecM` marker type class.
* If you are interpreting a free algebra into a context `F` with `foldMap`, you'll now need `F` to have an instance of the `RecursiveTailRecM` marker type class (in addition to the `Monad` instance).

If you run into any issues while updating, please get in touch on [Gitter](https://gitter.im/typelevel/cats).

### Changes

This release includes a fix for a bug in 0.6.0 (also fixed in 0.6.1):

* [#1062](https://github.com/typelevel/cats/pull/1062): `Order` instances for tuples are now lexicographic (instead of only comparing first elements)

And other bug fixes:

* [#1096](https://github.com/typelevel/cats/pull/1096): `inj` and `prj` on `Inject` now work consistently with respect to `null`

And some additions:

* [#1289](https://github.com/typelevel/cats/pull/1289) and [#1306](https://github.com/typelevel/cats/pull/1306): `EitherT` and improved `Either` syntax
* [#1280](https://github.com/typelevel/cats/pull/1280): `FlatMap` now has a `tailRecM` method
* [#1280](https://github.com/typelevel/cats/pull/1280): `RecursiveTailRecM` marker type class indicating that `tailRecM` is stack safe
* [#1266](https://github.com/typelevel/cats/pull/1266): `FreeT` monad transformer
* [#1225](https://github.com/typelevel/cats/pull/1225): `FunctorFilter` and `TraverseFilter`
* [#1121](https://github.com/typelevel/cats/pull/1121): `valueOr` and `merge` for `Validated`
* [#1188](https://github.com/typelevel/cats/pull/1188): `toValidatedNel` for `XorT`
* [#1127](https://github.com/typelevel/cats/pull/1127): `toTry` for `Xor`
* [#1269](https://github.com/typelevel/cats/pull/1269): `catchNonFatal` for `ApplicativeError`
* [#1130](https://github.com/typelevel/cats/pull/1130): `isEmpty` syntax method for `Monoid`
* [#1167](https://github.com/typelevel/cats/pull/1167): `minimum`, `maximum`, and related helper methods for `Foldable` and `Reducible`
* [#1243](https://github.com/typelevel/cats/pull/1243): `distinct` on `NonEmptyList` and `NonEmptyVector`
* [#1134](https://github.com/typelevel/cats/pull/1134): `cats.syntax.list` for à la carte list syntax imports
* [#1191](https://github.com/typelevel/cats/pull/1191): `cats.syntax.monoid` for à la carte `Monoid` syntax imports
* [#588](https://github.com/typelevel/cats/pull/588) and [#1063](https://github.com/typelevel/cats/pull/1063): `IdT`, the identity monad transformer
* [#1021](https://github.com/typelevel/cats/pull/1021) and [#1221](https://github.com/typelevel/cats/pull/1221): `Nested` (represents nested composition of type constructors)
* [#1172](https://github.com/typelevel/cats/pull/1172): `toNested` for `OptionT` and `XorT`
* [#1102](https://github.com/typelevel/cats/pull/1102) and [#1170](https://github.com/typelevel/cats/pull/1170): `Comparison` (represents the result of an `Order` comparison)
* [#1090](https://github.com/typelevel/cats/pull/1090): `Kleisli.lift`
* [#1169](https://github.com/typelevel/cats/pull/1169): `lift`, `inspect`, and related methods for `StateT`
* [#1114](https://github.com/typelevel/cats/pull/1114): `size` for `Foldable`
* [#1193](https://github.com/typelevel/cats/pull/1193): `reduceLeftM` for `Reducible`
* [#1097](https://github.com/typelevel/cats/pull/1097): Functor variance helpers (`widen` for `Functor` and `narrow` for `Contravariant`)
* [#1207](https://github.com/typelevel/cats/pull/1207): `tell` for `Writer` and `WriterT`, `value` for `Writer`
* [#1155](https://github.com/typelevel/cats/pull/1155): Convenience methods for constructing `XorT` values
* [#1085](https://github.com/typelevel/cats/pull/1085): `runTailRec` and `foldLeftM` for `Free`
* [#1299](https://github.com/typelevel/cats/pull/1299): `ContravariantCartesian` type class

And some name changes:

* [#1140](https://github.com/typelevel/cats/pull/1140): `cats.std` is now `cats.instances`
* [#1066](https://github.com/typelevel/cats/pull/1066), [#1068](https://github.com/typelevel/cats/pull/1068), [#1110](https://github.com/typelevel/cats/pull/1110), and [#1122](https://github.com/typelevel/cats/pull/1122): More unique type class instance names
* [#1072](https://github.com/typelevel/cats/pull/1072): `NaturalTransformation` is now `FunctionK`
* [#1085](https://github.com/typelevel/cats/pull/1085): `mapSuspension` on `Free` is now `compile`
* [#1111](https://github.com/typelevel/cats/pull/1111): `Free.Gosub` is now `Free.FlatMapped`
* [#1133](https://github.com/typelevel/cats/pull/1133): `Composite*` traits for binary type classes are renamed to `Composed*` for consistency (and are now private)

And other API changes:

* [#1231](https://github.com/typelevel/cats/pull/1231): `NonEmptyList` is now a case class instead of a type alias for a `OneAnd`
* [#1137](https://github.com/typelevel/cats/pull/1137): `NonEmptyVector` is now a value class instead of a type alias for a `OneAnd`
* [#1267](https://github.com/typelevel/cats/pull/1267): Overloaded variadic `apply` on `NonEmptyList` and `NonEmptyVector` is now `of`
* [#1234](https://github.com/typelevel/cats/pull/1234): `Applicative#pureEval` has been removed
* [#1202](https://github.com/typelevel/cats/pull/1202): `MonadFilter` no longer has a `filterM` method (see [#1225](https://github.com/typelevel/cats/pull/1225))
* [#1075](https://github.com/typelevel/cats/pull/1075): `foldMap` on `Free` now requires a `MonadRec` instance (instead of simply `Monad`)
* [#1085](https://github.com/typelevel/cats/pull/1085): `Free.suspend` no longer requires an `Applicative` instance
* [#1084](https://github.com/typelevel/cats/pull/1084): Safer `toString` for `Free` and `FreeApplicative`
* [#1100](https://github.com/typelevel/cats/pull/1100): Simplified constraints for methods on `Xor` and related types
* [#1171](https://github.com/typelevel/cats/pull/1171): Prioritization traits are now private

And many new instances:

* [#1059](https://github.com/typelevel/cats/pull/1059) and [#1147](https://github.com/typelevel/cats/pull/1147): `Monoid`, `MonadError`, and other instances for `scala.util.Try`
* [#1299](https://github.com/typelevel/cats/pull/1299): `Monad` for `Tuple2`
* [#1211](https://github.com/typelevel/cats/pull/1211): `Contravariant` for `Eq`
* [#1220](https://github.com/typelevel/cats/pull/1220): `Traverse` and `Comonad` for `Tuple2`
* [#1103](https://github.com/typelevel/cats/pull/1103): `Order`, `MonadError`, and other instances for `OptionT`
* [#1106](https://github.com/typelevel/cats/pull/1106): `Semigroup` and `Monoid` for `XorT`
* [#1138](https://github.com/typelevel/cats/pull/1138): `SemigroupK` and `MonadCombine` for `StateT`
* [#1128](https://github.com/typelevel/cats/pull/1128) `Semigroup` and `Monoid` for `Applicative`
* [#1049](https://github.com/typelevel/cats/pull/1049): `CoflatMap` for `WriterT`
* [#1076](https://github.com/typelevel/cats/pull/1076) and [#1261](https://github.com/typelevel/cats/pull/1261): `MonadRec` instances for `Eval`, `StateT`, and `Future`
* [#1105](https://github.com/typelevel/cats/pull/1105): `Unapply` instances for `Nested` shapes

And miscellaneous improvements to style and performance:

* [#1079](https://github.com/typelevel/cats/pull/1079): More consistent type lambdas
* [#1300](https://github.com/typelevel/cats/pull/1300): Much faster `Monoid` instances for `Map`

And improvements to the documentation:

* [#1145](https://github.com/typelevel/cats/pull/1145): Major rearrangements and additions
* [#1136](https://github.com/typelevel/cats/pull/1136): New chart for symbols
* [#1052](https://github.com/typelevel/cats/pull/1052): New "Why?" section
* [#1095](https://github.com/typelevel/cats/pull/1095), [#1226](https://github.com/typelevel/cats/pull/1226), and [#1227](https://github.com/typelevel/cats/pull/1227): New FAQ section
* [#1163](https://github.com/typelevel/cats/pull/1163): New import guide section
* [#1217](https://github.com/typelevel/cats/pull/1217), [#1223](https://github.com/typelevel/cats/pull/1223), and [#1239](https://github.com/typelevel/cats/pull/1239): New related projects
* [#1057](https://github.com/typelevel/cats/pull/1057) and [#1157](https://github.com/typelevel/cats/pull/1157): Copy-paste-friendly code blocks
* [#1104](https://github.com/typelevel/cats/pull/1104) and [#1115](https://github.com/typelevel/cats/pull/1115): Kitchen-sink imports in example code
* [#1050](https://github.com/typelevel/cats/pull/1050): Switch to [rouge](https://github.com/jneen/rouge) for syntax highlighting in the GitHub Pages site
* [#1119](https://github.com/typelevel/cats/pull/1119): Fix for `contramap` signature
* [#1141](https://github.com/typelevel/cats/pull/1141) and [#1162](https://github.com/typelevel/cats/pull/1162): Fixes for cats-kernel documentation
* [#1149](https://github.com/typelevel/cats/pull/1149): Spelling consistency for "type class"
* [#1183](https://github.com/typelevel/cats/pull/1183): More documentation about use of Machinist, Simulacrum, and kind-projector
* [#1056](https://github.com/typelevel/cats/pull/1056): Clarification about forgetful functors and the free monad
* [#1131](https://github.com/typelevel/cats/pull/1131) and [#1241](https://github.com/typelevel/cats/pull/1241): Simplified project structure listings
* [#1185](https://github.com/typelevel/cats/pull/1185), [#1186](https://github.com/typelevel/cats/pull/1186), and [#1189](https://github.com/typelevel/cats/pull/1189): Miscellaneous improvements for `Traverse` documentation

And the build:

* [#1159](https://github.com/typelevel/cats/pull/1159): Binary compatibility checking for cats-kernel via MiMa
* [#1256](https://github.com/typelevel/cats/pull/1256): More reliable Scala.js testing in Travis CI
* [#1123](https://github.com/typelevel/cats/pull/1123): cats-kernel is now included in the API documentation
* [#1051](https://github.com/typelevel/cats/pull/1051): Empty Scaladocs for 2.10 to avoid issues macros cause for API documentation generation on 2.10
* [#1154](https://github.com/typelevel/cats/pull/1154): Better POM hygiene: no Scoverage dependency
* [#1153](https://github.com/typelevel/cats/pull/1153) and [#1218](https://github.com/typelevel/cats/pull/1218): More consistent use of Simulacrum for syntax
* [#1093](https://github.com/typelevel/cats/pull/1093): Scalastyle is now aware of shared and Scala.js-specific source files
* [#1142](https://github.com/typelevel/cats/pull/1142): Additional formatting rules for Scalastyle
* [#1099](https://github.com/typelevel/cats/pull/1099): Type lambda style is now enforced by Scalastyle
* [#1258](https://github.com/typelevel/cats/pull/1258): Version updates for SBT and SBT plugins

We also welcome [Kailuo Wang](https://github.com/typelevel/cats/pull/1129), [Peter Neyens](https://github.com/typelevel/cats/pull/1179), and [Oscar Boykin](https://github.com/typelevel/cats/pull/1180) as new Cats maintainers!

## Version 0.6.1

> 2016 July 14

Version 0.6.1 is a patch release compatible with 0.6.0.

It contains one bug fix:

* [#1062](https://github.com/typelevel/cats/pull/1173/commits/8dd682771557274a61f1e773df0f999b44a9819d): Fixed a bug in the Order and PartialOrder instances for Tuple2+ where only the first element was used in comparisons

It also contains a change to the build:

* [#1173](https://github.com/typelevel/cats/pull/1173/commits/5531d1ac7a6807c1842cd4b5b599173b14b652a9): Add binary compatibility check to all published modules

## Version 0.6.0

> 2016 May 19

Version 0.6.0 is the sixth release.

Highlights of this release:

* [#990](https://github.com/typelevel/cats/pull/990):  Separate free package into its own module
* [#1001](https://github.com/typelevel/cats/pull/1001):  Introduce cats-kernel and remove algebra dependency

This release also includes some API changes:

* [#1046](https://github.com/typelevel/cats/pull/1046):  summon `ApplicativeErrorSyntax` for `F[_]` instead of `F[_, _]`
* [#1034](https://github.com/typelevel/cats/pull/1034):  Don't combine lefts on `Xor` and `XorT` `combine`
* [#1018](https://github.com/typelevel/cats/pull/1018):  Remove blocking (JVM-only) Future instances
* [#877](https://github.com/typelevel/cats/pull/877):  Remove required laziness in Prod, fixes #615


And additions:

* [#1032](https://github.com/typelevel/cats/pull/1032):  Added `Coproduct` `fold`
* [#1028](https://github.com/typelevel/cats/pull/1028):  Added `withFilter` for `OptionT`
* [#1014](https://github.com/typelevel/cats/pull/1014):  Added `Monoid` instance for `WriterT`
* [#1029](https://github.com/typelevel/cats/pull/1029):  Added an `ApplicativeError` instance for `Kleisli` and a `MonadError[Option, Unit]` to `std.option`
* [#1023](https://github.com/typelevel/cats/pull/1023):  Add `XorT#fromEither`
* [#984](https://github.com/typelevel/cats/pull/984):  Add `Validated.ensure`
* [#1020](https://github.com/typelevel/cats/pull/1020):  Add `Traverse.traverseM`


And some code improvements:

* [#1015](https://github.com/typelevel/cats/pull/1015):  Add `Apply.map2Eval` and allow traverse laziness
* [#1024](https://github.com/typelevel/cats/pull/1024):  Override reverse on reversed `PartialOrder` to return original instance
* [#880](https://github.com/typelevel/cats/pull/880):  Optimize `Eq[Vector[A]]` instance
* [#1019](https://github.com/typelevel/cats/pull/1019):  Use `Future#successful` in `pureEval` when possible

And bug fixes:

* [#1011](https://github.com/typelevel/cats/pull/1011):  Add missing type parameters.

And some other improvements to the organization documentation, tutorials, laws and tests, including:

* [#1045](https://github.com/typelevel/cats/pull/1045):  Add a link to the `OptionT` documentation from the monad docs.
* [#1043](https://github.com/typelevel/cats/pull/1043):  Add notes about kind-projector usage in docs
* [#1042](https://github.com/typelevel/cats/pull/1042):  Cats 0.5.0 no longer pre-release
* [#1036](https://github.com/typelevel/cats/pull/1036):  Add FPiS to the "Resources for Learners" section
* [#1035](https://github.com/typelevel/cats/pull/1035):  Run kernel-law tests for JS as part of build
* [#991](https://github.com/typelevel/cats/pull/991):  Replace `~>` with `NaturalTransformation`
* [#1027](https://github.com/typelevel/cats/pull/1027):  Remove unnecessary `nelSemigroup` from `traverse` doc
* [#1022](https://github.com/typelevel/cats/pull/1022):  Add law-checking for `asMeetPartialOrder` and `asJoinPartialOrder`
* [#990](https://github.com/typelevel/cats/pull/990):  Separate free package into its own module


## Version 0.5.0

> 2016 April 28

Version 0.5.0 is the fifth release.

This release includes some API changes:

`cats.laws.discipline.eq` no longer provides `Eq` instances for `Tuple2` and `Tuple3`, these instances and together with some other new instances for `Tuple`s are now provided by `cats.std.tuple` (through inheriting the instance trait defined in algebra 0.4.2).

* [#910](https://github.com/typelevel/cats/pull/910): Remove `Streaming` and `StreamingT`
* [#967](https://github.com/typelevel/cats/pull/967): `product` and `map` can be implemented in terms of `ap`
* [#970](https://github.com/typelevel/cats/pull/970): Renamed `Kleisli#apply`to `ap`
* [#994](https://github.com/typelevel/cats/pull/994): updated to latest algebra (brought in all the new goodies)

And additions:

* [#853](https://github.com/typelevel/cats/pull/853): Adds a new `LiftTrans` type class
* [#864](https://github.com/typelevel/cats/pull/864): Add `Bifoldable`
* [#875](https://github.com/typelevel/cats/pull/875): Add `.get` method to `StateT`
* [#884](https://github.com/typelevel/cats/pull/884): Add `Applicative` syntax
* [#886](https://github.com/typelevel/cats/pull/886): Add `map` method to `OneAnd`
* [#927](https://github.com/typelevel/cats/pull/927): `XorT.ensure` method
* [#925](https://github.com/typelevel/cats/pull/925): Stack-safe `foldM`
* [#922](https://github.com/typelevel/cats/pull/922): Add `tell` and `writer` syntax for creating `Writers`.
* [#903](https://github.com/typelevel/cats/pull/903): Add `Bitraverse`
* [#928](https://github.com/typelevel/cats/pull/928): Add missing `Show` instances
* [#940](https://github.com/typelevel/cats/pull/940): More flexible `TransLift`
* [#946](https://github.com/typelevel/cats/pull/946): Added `OptionT.none`
* [#947](https://github.com/typelevel/cats/pull/947): Syntax for `ApplicativeError`
* [#971](https://github.com/typelevel/cats/pull/971): Add `toValidatedNel` to `Xor`
* [#973](https://github.com/typelevel/cats/pull/973): Add `flatMapF` for `StateT`
* [#985](https://github.com/typelevel/cats/pull/985): Add object `reducible` for reducible syntax
* [#996](https://github.com/typelevel/cats/pull/996): Add `SemigroupK` instance for `Xor`
* [#998](https://github.com/typelevel/cats/pull/998): Add `SemigroupK` instance for `Validated`
* [#986](https://github.com/typelevel/cats/pull/986): Add `Bitraverse` instances for `Validated` and `XorT`


And bug fixes:

* [#873](https://github.com/typelevel/cats/pull/873): Fix `OptionIdOps.some` to always return `Some`
* [#958](https://github.com/typelevel/cats/pull/958): Switch off scaladoc generation for Scala 2.10 due to macro problems
* [#955](https://github.com/typelevel/cats/pull/955): Rename `Id` instances to `idInstances` to make selective import easier


And removals:

* [#910](https://github.com/typelevel/cats/pull/910): Remove `Streaming` and `StreamingT`


And some other improvements to the documentation, tutorials, laws and tests, including:

* [#880](https://github.com/typelevel/cats/pull/880): Optimize `Eq[Vector[A]]` instance
* [#878](https://github.com/typelevel/cats/pull/878): Fix bug in freemonad doc
* [#870](https://github.com/typelevel/cats/pull/870): Fixed doc string for `StateT`'s `runEmptyA()`
* [#866](https://github.com/typelevel/cats/pull/866): Add some tests for `Coproduct` and `WriterT`
* [#883](https://github.com/typelevel/cats/pull/883): Delegate to `Traverse.sequence` in `Applicative.sequence`
* [#893](https://github.com/typelevel/cats/pull/893): Add `Reducible` laws
* [#923](https://github.com/typelevel/cats/pull/923): Make `Call.loop` `@tailrec` optimized
* [#916](https://github.com/typelevel/cats/pull/916): add `-P:scalajs:mapSourceURI` option
* [#909](https://github.com/typelevel/cats/pull/909): Make `Bifunctor` universal
* [#905](https://github.com/typelevel/cats/pull/905): make `Unapply` serializable
* [#902](https://github.com/typelevel/cats/pull/902): Make table in `Kleisli` readable
* [#897](https://github.com/typelevel/cats/pull/897): Add `Prod` tests
* [#938](https://github.com/typelevel/cats/pull/938): Onward to scala 2.11.8
* [#941](https://github.com/typelevel/cats/pull/941): Type class composition and `MonadState` tests
* [#949](https://github.com/typelevel/cats/pull/949): Add .ensime_cache to gitignore
* [#954](https://github.com/typelevel/cats/pull/954): Switch to use nodeJsEnv as default jsEnv to build scala.js
* [#956](https://github.com/typelevel/cats/pull/956): Upgrade scala.js from 0.6.7 -> 0.6.8
* [#960](https://github.com/typelevel/cats/pull/960): More `Reducible` tests
* [#962](https://github.com/typelevel/cats/pull/962): Improving test coverage
* [#964](https://github.com/typelevel/cats/pull/964): Clarify stabilty guarantees; drop 'proof of concept' and 'experimental'
* [#972](https://github.com/typelevel/cats/pull/972): Fix swapped f and g in `invariant` docs
* [#979](https://github.com/typelevel/cats/pull/979): Fix outdated import for `cats.syntax.apply._`
* [#995](https://github.com/typelevel/cats/pull/995): Move coverage away from bash
* [#1002](https://github.com/typelevel/cats/pull/1002): Correct the URL for *Data types à la carte*
* [#1005](https://github.com/typelevel/cats/pull/1005): fix broken link in foldable docs


As always thanks to everyone who filed issues, participated in the Cats Gitter
channel, submitted code, or helped review pull requests.



## Version 0.4.1

> 2016 February 4

Version 0.4.1 is a patch release in the 0.4 series and is binary compatible with
version 0.4.0.

This patch fixes bugs with the `dropWhile` methods on `Streaming` and
`StreamingT`.

This release corrects outdated build/POM metadata, which should fix API doc URLS.

Bug fixes:

* [#856](https://github.com/typelevel/cats/pull/856): Fix `Streaming` and `StreamingT` `dropWhile` functions

Build/publishing changes:

* [#852](https://github.com/typelevel/cats/pull/852) Update build with org change

Documentation and site improvements:

* [#859](https://github.com/typelevel/cats/pull/859) Add Contravariant documentation page
* [#861](https://github.com/typelevel/cats/pull/861) Docs: Revive useful links section. Update URLs

## Version 0.4.0

> 2016 February 1

Version 0.4.0 is the fourth release of the Cats library, and the first release
published under the `org.typelevel` group from the
[Typelevel](https://github.com/typelevel) organization on GitHub (previous
releases had been published to `org.spire-math` from `non/cats`). This means
that users will need to change the `groupId` for their Cats dependencies when
updating. If you have a line like this in your SBT build configuration, for
example:

```scala
libraryDependencies += "org.spire-math" %% "cats" % "0.3.0"
```

You will need to change it to the following:

```scala
libraryDependencies += "org.typelevel" %% "cats" % "0.4.0"
```

This release no longer includes `cats-state` or `cats-free` artifacts, since
the `cats.state` and `cats.free` packages have been moved into `cats-core`.

If you've checked out the GitHub repository locally, it would be a good idea to
update your remote to point to the new organization, which will typically look
like this (note that you should confirm that `origin` is the appropriate
remote name):

```bash
git remote set-url origin git@github.com:typelevel/cats.git
```

This release includes a large number of breaking changes, including most
prominently the introduction of a new `Cartesian` type class that is a supertype
of `Monad` (and many other types). If you use the `|@|` syntax that had
previously been provided by `Apply`, you'll need to change your imports from
`cats.syntax.apply._` to `cats.syntax.cartesian._`. For example:

```scala
scala> import cats.Eval, cats.syntax.cartesian._
import cats.Eval
import cats.syntax.cartesian._

scala> (Eval.now("v") |@| Eval.now(0.4)).tupled
res0: cats.Eval[(String, Double)] = cats.Eval$$anon$5@104f8bbd
```

Other changes in this release are described below.

This version includes API changes:

* [#555](https://github.com/typelevel/cats/pull/555): `|@|` syntax is now
  provided by `cats.syntax.cartesian`
* [#835](https://github.com/typelevel/cats/pull/835): `State` and `StateT` are
  now in the `cats.data` package
* [#781](https://github.com/typelevel/cats/pull/781): `combine` on `SemigroupK`
  is now `combineK`
* [#821](https://github.com/typelevel/cats/pull/821) and
  [#833](https://github.com/typelevel/cats/pull/833): The order of arguments for
  `ap` has been reversed (now function first)
* [#833](https://github.com/typelevel/cats/pull/833): `ap` on
  `CartesianBuilderN` is now `apWith`
* [#782](https://github.com/typelevel/cats/pull/782): `State` now uses `Eval`
  instead of `Trampoline` for stack safety
* [#697](https://github.com/typelevel/cats/pull/697): `or` for natural
  transformations is now an instance method
* [#725](https://github.com/typelevel/cats/pull/725): `orElse` on `XorT` and
  does not unnecessarily constrain the type of the left side of the result
* [#648](https://github.com/typelevel/cats/pull/648): Some types now extend
  `Product` and `Serializable` to improve type inference
* [#647](https://github.com/typelevel/cats/pull/647): `ProdInstancesN` names
  changed for consistency
* [#636](https://github.com/typelevel/cats/pull/636): `Eval` is now
  `Serializable`
* [#685](https://github.com/typelevel/cats/pull/685): Fixes for copy-paste
  errors in method names for instances for `Validated`
* [#778](https://github.com/typelevel/cats/pull/778): Unnecessary type parameter
  on `Foldable`'s `sequence_` has been removed

And additions:

* [#555](https://github.com/typelevel/cats/pull/555) and
  [#795](https://github.com/typelevel/cats/pull/795): `Cartesian`
* [#671](https://github.com/typelevel/cats/pull/671): `Coproduct` and `Inject`
* [#812](https://github.com/typelevel/cats/pull/812): `ApplicativeError`
* [#765](https://github.com/typelevel/cats/pull/765): `State` and `Free` (and
  related types) are now in the core module
* [#611](https://github.com/typelevel/cats/pull/611): `Validated` now has an
  `andThen` method that provides binding (but without the `for`-comprehension
  syntactic sugar that the name `flatMap` would bring)
* [#796](https://github.com/typelevel/cats/pull/796): `sequenceU_` and
  `traverseU_` on `Foldable`
* [#780](https://github.com/typelevel/cats/pull/780): `transformS` for `StateT`
* [#807](https://github.com/typelevel/cats/pull/807): `valueOr` for `XorT`
* [#714](https://github.com/typelevel/cats/pull/714): `orElse` for `XorT`
* [#705](https://github.com/typelevel/cats/pull/705): `getOrElseF` for `XorT`
* [#731](https://github.com/typelevel/cats/pull/731): `swap` for `Validated`
* [#571](https://github.com/typelevel/cats/pull/571): `transform` and
  `subflatMap` on `OptionT` and `XorT`
* [#757](https://github.com/typelevel/cats/pull/757) and
  [#843](https://github.com/typelevel/cats/pull/843): `compose` for
  `Alternative` and `composeK` for `MonoidK`
* [#667](https://github.com/typelevel/cats/pull/667): `OptionT.liftF`

And removals:

* [#613](https://github.com/typelevel/cats/pull/613): `Free` and
  `FreeApplicative` constructors are now private
* [#605](https://github.com/typelevel/cats/pull/605): `filter` on `Validated`
* [#698](https://github.com/typelevel/cats/pull/698): `MonadCombine` instances
  for `OptionT`
* [#635](https://github.com/typelevel/cats/pull/635): `Kleisli`'s redundant
  `lmap`, which was equivalent to `local`
* [#752](https://github.com/typelevel/cats/pull/752): `Cokleisli.cokleisli`,
  which was equivalent to `Cokleisli.apply`
* [#687](https://github.com/typelevel/cats/pull/687): Unused `XorTMonadCombine`
* [#622](https://github.com/typelevel/cats/pull/622): Many prioritization types
  are now private

And new type class instances:

* [#644](https://github.com/typelevel/cats/pull/644): `Traverse` and `Foldable`
  instances for `XorT`
* [#691](https://github.com/typelevel/cats/pull/691): Various instances for
  `Function1`
* [#628](https://github.com/typelevel/cats/pull/628) and
  [#696](https://github.com/typelevel/cats/pull/696): Various instances for
  `WriterT`
* [#673](https://github.com/typelevel/cats/pull/673): `Bifunctor` instances for
  `WriterT`
* [#715](https://github.com/typelevel/cats/pull/715) and
  [#716](https://github.com/typelevel/cats/pull/716): `Semigroup` and `Monoid`
  instances for `Validated`
* [#717](https://github.com/typelevel/cats/pull/717) and
  [#718](https://github.com/typelevel/cats/pull/718): `Semigroup` instances for
  `Xor` and `Const`
* [#818](https://github.com/typelevel/cats/pull/818): `CoflatMap` instance for
  `Vector`
* [#626](https://github.com/typelevel/cats/pull/626): `Contravariant` instances
  for `Const` and `Kleisli`
* [#621](https://github.com/typelevel/cats/pull/621): `Id` instances for
  `Kleisli`
* [#772](https://github.com/typelevel/cats/pull/772): `Reducible` instances for
  `OneAnd`
* [#816](https://github.com/typelevel/cats/pull/816): `Traverse` instances for
  `OneAnd`
* [#639](https://github.com/typelevel/cats/issues/639): `Traverse` instance
  for `Id`
* [#774](https://github.com/typelevel/cats/pull/774) and
  [#775](https://github.com/typelevel/cats/pull/775): `Show` instances for
  `Vector` and `Stream`

And bug fixes:

* [#623](https://github.com/typelevel/cats/pull/623) fixes
  [#563](https://github.com/typelevel/cats/issues/563), a bug in the behavior of
  `dropWhile_` on `Foldable`
* [#665](https://github.com/typelevel/cats/pull/665) fixes
  [#662](https://github.com/typelevel/cats/pull/662), a bug that resulted in
  re-evaluation after memoization in `Streaming`
* [#683](https://github.com/typelevel/cats/pull/683) fixes
  [#677](https://github.com/typelevel/cats/issues/677), a bug in
  `Streaming.thunk`
* [#801](https://github.com/typelevel/cats/pull/801): Fixes order effect bug in
  `foldMap` on `FreeApplicative`
* [#798](https://github.com/typelevel/cats/pull/798): Fixes bug in `filter` on
  `StreamingT`
* [#656](https://github.com/typelevel/cats/pull/656): Fixes bug in `drop` on
  `StreamingT`
* [#769](https://github.com/typelevel/cats/pull/769): Improved stack consumption
  for `Eval.Call`

And some dependency updates:

* [#833](https://github.com/typelevel/cats/pull/833): Update to Simulacrum
  0.7.0
* [#764](https://github.com/typelevel/cats/pull/764): 2.10 version is now
  2.10.6
* [#643](https://github.com/typelevel/cats/pull/643): Update to Catalysts 0.2.0
* [#727](https://github.com/typelevel/cats/pull/727): Update to Scalastyle 0.8.0

There are also many improvements to the documentation, tutorials, laws, tests,
and benchmarks, including the following:

* [#724](https://github.com/typelevel/cats/pull/724): sbt-doctest is now used to
  validate Scaladoc examples
* [#806](https://github.com/typelevel/cats/pull/806): Various improvements to
  use of Simulacrum, which is now a compile-time-only dependency
* [#734](https://github.com/typelevel/cats/pull/734): Documentation on testing
  conventions
* [#710](https://github.com/typelevel/cats/pull/710): Documentation for
  `Invariant`
* [#832](https://github.com/typelevel/cats/pull/832): Updated `Free`
  documentation
* [#824](https://github.com/typelevel/cats/pull/824): New examples for
  `Foldable`
* [#797](https://github.com/typelevel/cats/pull/797): Scaladoc examples for
  methods on `Arrow`
* [#783](https://github.com/typelevel/cats/pull/783) and others: Scaladoc
  examples for syntax methods
* [#720](https://github.com/typelevel/cats/pull/720): Expanded documentation for
  `FreeApplicative`
* [#636](https://github.com/typelevel/cats/pull/636): Law checking for `Eval`
* [#649](https://github.com/typelevel/cats/pull/649) and
  [#660](https://github.com/typelevel/cats/pull/660): Better `Arbitrary`
  instances for `Streaming` and `StreamingT`
* [#722](https://github.com/typelevel/cats/pull/722): More consistent `toString`
  for `StreamingT`
* [#672](https://github.com/typelevel/cats/pull/672): Additional laws for
  `Profunctor`
* [#668](https://github.com/typelevel/cats/pull/668),
  [#669](https://github.com/typelevel/cats/pull/669),
  [#679](https://github.com/typelevel/cats/pull/679),
  [#680](https://github.com/typelevel/cats/pull/680), and
  [#681](https://github.com/typelevel/cats/pull/681): Additional law checking
  for `Xor`, `XorT`, and `Either`
* [#707](https://github.com/typelevel/cats/pull/707): Additional testing for
  `State` and `StateT`
* [#736](https://github.com/typelevel/cats/pull/736): `map` / `flatMap`
  coherence
* [#748](https://github.com/typelevel/cats/pull/748): Left and right identity
  laws for `Kleisli`
* [#753](https://github.com/typelevel/cats/pull/753): Consistency tests for
  `Cokleisli`
* [#733](https://github.com/typelevel/cats/pull/733): Associativity laws for
  `Kleisli` and `Cokleisli` composition
* [#741](https://github.com/typelevel/cats/pull/741): Tests for
  `Unapply`-supported syntax
* [#690](https://github.com/typelevel/cats/pull/690): Error reporting
  improvements for serializability tests
* [#701](https://github.com/typelevel/cats/pull/701): Better documentation for
  the Travis CI script
* [#787](https://github.com/typelevel/cats/pull/787): Support for cross-module
  Scaladoc links

Known issues:

* [#702](https://github.com/typelevel/cats/pull/702): This change identified and
  fixed a stack safety bug in `foldMap` on `Free`, but raised other issues (see
  [#712](https://github.com/typelevel/cats/issues/712)) and was reverted in
  [#713](https://github.com/typelevel/cats/pull/713);
  [#721](https://github.com/typelevel/cats/issues/721) now tracks the non-stack
  safety of `Free`'s `foldMap`

As always thanks to everyone who filed issues, participated in the Cats Gitter
channel, submitted code, or helped review pull requests.

## Version 0.3.0

> 2015 November 8

Version 0.3.0 is the third release of the Cats library.

This version includes new type class instances:

* [#545](https://github.com/typelevel/cats/pull/545): `Semigroup` instances for
  `OneAnd`
* [#521](https://github.com/typelevel/cats/pull/521): `Monoid` instances for `Xor`
  when the left side has a `Semigroup` instance and the right side has a
  `Monoid`
* [#497](https://github.com/typelevel/cats/pull/497): `Monoid` instances for `Set`
* [#559](https://github.com/typelevel/cats/pull/559): `Bifunctor` instances for
  `Validated`, `Ior`, `Xor`, and `XorT`
* [#569](https://github.com/typelevel/cats/pull/569): `Functor` instances for
  `OptionT` when `F` has a `Functor` instance but not a `Monad`
* [#600](https://github.com/typelevel/cats/pull/600): `Show` instances for `Option`
  and `OptionT`
* [#601](https://github.com/typelevel/cats/pull/601): `Show` instances for `List`
* [#602](https://github.com/typelevel/cats/pull/602): `Show` instances for `Set`
* [#568](https://github.com/typelevel/cats/pull/568): Several new `Unapply` shapes

And API changes:

* [#592](https://github.com/typelevel/cats/pull/592): `fromTryCatch` on `Xor` and
  `Validated` is now `catchOnly`
* [#553](https://github.com/typelevel/cats/pull/553): `MonadError` now characterizes
  type constructors of kind `* -> *` instead of `(*, *) -> *`
* [#598](https://github.com/typelevel/cats/pull/598): `OneAnd`'s type constructor type
  parameter is now before the element type
* [#610](https://github.com/typelevel/cats/pull/610): `XorT`'s `toOption` returns an
  `OptionT[F, B]` instead of an `F[Option[B]]`
* [#518](https://github.com/typelevel/cats/pull/518): `Free`'s `resume` method now
  returns an `Xor` instead of an `Either`
* [#575](https://github.com/typelevel/cats/pull/575) and
  [#606](https://github.com/typelevel/cats/pull/606): `orElse` on `Xor` and
  `Validated` does not unnecessarily constrain the type of the left side of the
  result
* [#577](https://github.com/typelevel/cats/pull/577): `*Aux` helper classes have been
  renamed `*PartiallyApplied`

And additions:

* [#542](https://github.com/typelevel/cats/pull/542): `WriterT`
* [#567](https://github.com/typelevel/cats/pull/567): `Ior.fromOptions`
* [#528](https://github.com/typelevel/cats/pull/528): `OptionT.fromOption`
* [#562](https://github.com/typelevel/cats/pull/562): `handleErrorWith` and related
  helper methods on `MonadError`
* [#520](https://github.com/typelevel/cats/pull/520): `toNel` and `fromList`
  conversions from `List` to `NonEmptyList`
* [#533](https://github.com/typelevel/cats/pull/533): Conversions between types with
  `Foldable` instances and `Streaming`
* [#507](https://github.com/typelevel/cats/pull/507): `isJvm` and `isJs` macros in the
  new `cats.macros.Platform`
* [#572](https://github.com/typelevel/cats/pull/572): `analyze` on `FreeApplicative`
  for compilation into a `Monoid`
* [#587](https://github.com/typelevel/cats/pull/587): Syntax for lifting values (and
  optional values) into `Validated`

And several aliases:

* [#492](https://github.com/typelevel/cats/pull/492): `FlatMapSyntax` now includes
  `followedBy`, which is an alias for `>>`, together with a new
  `followedByEval`, which allows the caller to choose the evaluation strategy of
  the second action
* [#523](https://github.com/typelevel/cats/pull/523): `Foldable` now has a
  `combineAll` method that aliases `fold` and allows postfix usage via
  `FoldableSyntax`

And a few removals:

* [#524](https://github.com/typelevel/cats/pull/524): `FreeApplicative`'s redundant
  `hoist`, which was equivalent to `compile`
* [#531](https://github.com/typelevel/cats/pull/531): `Coyoneda`'s `by`
* [#612](https://github.com/typelevel/cats/pull/612): Many prioritization and instance
  traits are now private

And bug fixes:

* [#547](https://github.com/typelevel/cats/pull/547): The `empty` values for
  `Monoid[Double]` and `Monoid[Float]` are now `0` instead of `1`
* [#530](https://github.com/typelevel/cats/pull/530): `Streaming.take(n).toList` no
  longer evaluates the `n + 1`-st element
* [#538](https://github.com/typelevel/cats/pull/538): `OneAnd`'s instances are
  properly prioritized

There are also many improvements to the documentation, tutorials, laws, tests,
and benchmarks:

* [#522](https://github.com/typelevel/cats/pull/522): ScalaTest's `===` now uses `Eq`
  instances
* [#502](https://github.com/typelevel/cats/pull/502): `Traverse`'s laws verify the
  consistency of `foldMap` and `traverse`
* [#519](https://github.com/typelevel/cats/pull/519): Benchmarks (and performance
  improvements) for `Eval`
* …and many others

Thanks to everyone who filed issues, participated in the Cats Gitter channel,
submitted code, or helped review pull requests.

## Version 0.2.0

> 2015 August 31

Version 0.2.0 is the second release of the Cats library.

The most exciting feature of this release is Scala.js support, which
comes courtesy of much hard work by the Scala.js community (especially
Alistair Johnson). The SBT build configuration and project layout were
updated to support building for both the JVM and JS platforms.

Since the 0.1.2 release there was wide agreement that the split
between `cats-core` and `cats-std` was awkward. The two projects have
been combined into `cats-core`, meaning that type class instances for
common types like `List` are now available in `cats-core`.

There was also a concerted effort to improve and add documentation to
the project. Many people helped find typos, broken links, and places
where the docs could be improved. In particular, the following
tutorials were added or overhauled:

 * `Applicative`
 * `Const`
 * `Foldable`
 * `Free`
 * `FreeApplicative`
 * `Kleisli`
 * `Monad`
 * `Monoid`
 * `Semigroup`
 * `SemigroupK`
 * `Traverse`
 * `Validated`
 * `Xor`

Several new type classes and data types were introduced:

 * `Choice[F[_, _]]`
 * `Group[A]`
 * `MonadReader[F[_, _], R]`
 * `Streaming[A]` and `StreamingT[F[_], A]`
 * `Prod[F[_], G[_], A]` and `Func[F[_], A, B]`

Syntax tests were added to ensure that existing syntax worked, and
there has been some movement to enrich existing types with syntax to
make converting them to Cats types easier.

The previous `Fold[A]` type, which was used to support lazy folds, has
been replaced with `Eval[A]`. This type supports both strict and lazy
evaluation, supports lazy `map` and `flatMap`, and is trampolined for
stack safety. The definition of `Foldable#foldRight` has been updated
to something much more idiomatic and easier to reason about. The goal
is to support laziness in Cats via the `Eval[A]` type wherever
possible.

In addition to these specific changes there were numerous small bug
fixes, additions, improvements, and updates. Thanks to everyone who
filed issues, participated in the Cats Gitter channel, submitted code,
or helped review pull requests.

## Version 0.1.2

> 2015 July 17

(Due to problems with publishing 0.1.0 and 0.1.1 are incomplete.)

Version 0.1.2 is the first non-snapshot version of the Cats library!
It is intended to assist the creation of dependent libraries and to be
an early look at Cats' design.

Much of the library is quite mature, but there are no source- or
binary-compatibility guarantees at this time. The overarching design
of the library is still somewhat in flux, although mostly we expect
there will be new type classes, instances, and syntax. Some package
and module boundaries may also shift.

For complete credits, see [AUTHORS.md](AUTHORS.md) for a list of
people whose work has made this release possible.
