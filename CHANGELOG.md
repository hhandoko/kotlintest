Changelog
=========

This project follows [semantic versioning](http://semver.org/).

Version 1.3.2, released 2016-06-05
----------------------------------

### Changed

* Added `a shouldBe exactly(b)` matcher for doubles

* `kotlintest` only pulls in `mockito-core` now instead of `mockito-all`


Version 1.3.1, released 2016-06-03
----------------------------------

### Changed

* Bumped Kotlin version to 1.0.3

Version 1.3.0, released 2016-06-03
----------------------------------

[Closed Issues](https://github.com/kotlintest/kotlintest/issues?utf8=%E2%9C%93&q=is%3Aclosed+milestone%3A2.0)

### Added

* StringSpec. You can use simply use Strings as the basis for tests, eg:

```kotlin
class StringSpecExample : StringSpec() {
  init {
    "strings.size should return size of string" {
      "hello".length shouldBe 5
      "hello" should haveLength(5)
    }

    "strings should support config" {
      "hello".length shouldBe 5
    }.config(invocations = 5)
  }
}
```

* Table Tests. Tables allow you to manually specific combinations of values that should be used, and are useful for 
edge cases and other specific values you want to test. The headers are used for when values fail, 
the output can show you what inputs were used for what labels. An example of using a table consisting of two-value tuples:

```kotlin
class TableExample : StringSpec(), TableTesting {
  init {
    "numbers should be prime" {
      val table = table(
          headers("a", "b"),
          row(5, 5),
          row(4, 6),
          row(3, 7)
      )
      forAll(table) { a, b ->
        a + b == 10
      }
    }
  }
}
```

* Property tests. Property tests automatically generate values for testings. You provide, or have KotlinTest provide for you, `generators`, which will generate a set of values and the unit test will be executed for each of those values. An example using two strings and asserting that the lengths are correct:

```kotlin
class PropertyExample: StringSpec() {

  "String size" {
    forAll({ a: String, b: String ->
      (a + b).length == a.length + b.length
    })
  }

}
```

That test will be executed 100 times with random values in each test. See more in the readme.

* autoClose. Fields of type `Closeable` can be registered for automatic resource closing:

```kotlin
class StringSpecExample : StringSpec() {
  val reader = autoClose(StringReader("xyz"))
  
  ...
}
```

* `haveLength` matcher. You can now write for strings:

```kotlin
someString should haveLength(10)
```


* `haveSize` matcher. You can now write for collections:

```kotlin
myCollection should haveSize(4)
```

* `contain` matcher. You can now write

```kotlin
val col = listOf(1,2,3,4,5)
col should contain(4)
```

* `containInAnyOrder` matcher. You can now write

```kotlin
val col = listOf(1,2,3,4,5)
col should containInAnyOrder(4,2,3)
```

* `haveKey` Map<K,V> matcher. You can now write

```kotlin
val map = mapOf(Pair(1, "a"), Pair(2, "b"))
map should haveKey(1)
```

* `haveValue` Map<K,V> matcher. You can now write

```kotlin
val map = mapOf(Pair(1, "a"), Pair(2, "b"))
map should haveValue("a")
```

* `contain` Map<K,V> matcher. You can now write

```kotlin
val map = mapOf(Pair(1, "a"), Pair(2, "b"))
map should contain(1, "a")
```

* `beTheSameInstanceAs` reference matcher. This is an alias for `x should be theSameInstanceAs(y)`, allowing `x should beTheSameInstanceAs(y)` which fits in with new matcher style.

### Changed

#### Replaced `timeout` + `timeUnit` with `Duration` ([#29](https://github.com/kotlintest/kotlintest/issues/29))

You can now write `config(timeout = 2.seconds)` instead of 
`config(timeout = 2, timeoutUnit = TimeUnit.SECONDS)`.

### Deprecated

nothing

### Removed

nothing

### Fixed

* Ignored tests now display properly. https://github.com/kotlintest/kotlintest/issues/43
* Failing tests reported as a success AND a failure https://github.com/kotlintest/kotlintest/issues/42
