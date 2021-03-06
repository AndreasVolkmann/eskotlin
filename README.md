# ES Kotlin
Elasticsearch Query DSL for Kotlin.

This library aims to minimize the gap between the Elasticsearch JSON query DSL, and the API used when writing kotlin applications.
This integrates with the existing java API, only providing a nicer syntax to build the queries.


# Version

For Elastic Search version 2.2.x, use ES Kotlin 0.1.0

For Elastic Search version 5.0.x use ES Kotlin 0.3.0

# Getting Started

## Gradle

```
repositories {
    mavenCentral()
    ...
    maven {
        url  "http://dl.bintray.com/mbuhot/maven"
   }
}
dependencies {
    compile 'mbuhot:eskotlin:0.3.0'
    ...
}
```

## Maven

Full details on [bintray](https://bintray.com/mbuhot/maven/eskotlin/)

```
<dependency>
  <groupId>mbuhot</groupId>
  <artifactId>eskotlin</artifactId>
  <version>0.3.0</version>
  <type>pom</type>
</dependency>
```


# Examples

## Term Query

JSON:
```json
{
    "term" : { "user" : "Kimchy" }
}
```

Kotlin:
```kotlin
val query = term {
    "user" to "Kimchy"
}
```

## Bool Query

JSON:
```json
{
    "bool" : {
        "must" : {
            "term" : { "user" : "kimchy" }
        },
        "filter": {
            "term" : { "tag" : "tech" }
        },
        "must_not" : {
            "range" : {
                "age" : { "from" : 10, "to" : 20 }
            }
        },
        "should" : [
            {
                "term" : { "tag" : "wow" }
            },
            {
                "term" : { "tag" : "elasticsearch" }
            }
        ],
        "minimum_should_match" : 1,
        "boost" : 1.0
    }
}
```

Kotlin:
```kotlin
val query = bool {
    must {
        term { "user" to "kimchy" }
    }
    filter {
        term { "tag" to "tech" }
    }
    must_not {
        range {
            "age" to {
                from = 10
                to = 20
            }
        }
    }
    should = listOf(
        term { "tag" to "wow" },
        term { "tag" to "elasticsearch" })
    minimum_should_match = 1
    boost = 1.0f
}

```

## Function Score Query

JSON:
```json
{
    "function_score": {
        "query": {
            "match_all": {}
        },
        "functions": [
            {
                "filter": {
                    "term": {
                        "foo": "bar"
                    }
                },
                "gauss": {
                    "baz": {
                        "scale": 1.0
                    }
                }
            },
            {
                "filter": {
                    "match_all": {}
                },
                "random_score": {
                    "seed": 234
                }
            },
            {
                "exp": {
                    "qux": {
                        "scale": 2.3
                    }
                }
            }
        ],
        "score_mode": "max",
        "boost_mode": "multiply",
        "max_boost": 5.0,
        "boost": 1.2,
        "min_score": 0.001
    }
}
```

Kotlin:
```kotlin
val query = function_score {
    query = match_all { }
    functions = listOf(
        term { "foo" to "bar" } to gaussDecayFunction("baz", 1.0),
        match_all { } to randomFunction(234L),
        null to exponentialDecayFunction("qux", 2.3))

    boost = 1.2f
    boost_mode = "multiply"
    score_mode = "max"
    max_boost = 5.0f
    min_score = 0.001f
}
```

See the src/test directory for more examples.


# API Coverage

## Full Text Queries - Done

+ [Match](./src/test/kotlin/mbuhot/eskotlin/query/fulltext/MatchTest.kt)
+ [Multi Match](./src/test/kotlin/mbuhot/eskotlin/query/fulltext/MultiMatchTest.kt)
+ [Common Terms](./src/test/kotlin/mbuhot/eskotlin/query/fulltext/CommonTest.kt)

## Term Queries - Done

+ [Term](./src/test/kotlin/mbuhot/eskotlin/query/term/TermTest.kt)
+ [Terms](./src/test/kotlin/mbuhot/eskotlin/query/term/TermsTest.kt)
+ [Range](./src/test/kotlin/mbuhot/eskotlin/query/term/RangeTest.kt)
+ [Exists](./src/test/kotlin/mbuhot/eskotlin/query/term/ExistsTest.kt)
+ [Prefix](./src/test/kotlin/mbuhot/eskotlin/query/term/PrefixTest.kt)
+ [Wildcard](./src/test/kotlin/mbuhot/eskotlin/query/term/WildcardTest.kt)
+ [Regex](./src/test/kotlin/mbuhot/eskotlin/query/term/RegexTest.kt)
+ [Fuzzy](./src/test/kotlin/mbuhot/eskotlin/query/term/FuzzyTest.kt)
+ [Type](./src/test/kotlin/mbuhot/eskotlin/query/term/TypeTest.kt)
+ [Ids](./src/test/kotlin/mbuhot/eskotlin/query/term/IdsTest.kt)

## Compound Queries - Done

+ [Constant Score](./src/test/kotlin/mbuhot/eskotlin/query/compound/ConstantScoreTest.kt)
+ [Bool](./src/test/kotlin/mbuhot/eskotlin/query/compound/BoolTest.kt)
+ [Dis Max](./src/test/kotlin/mbuhot/eskotlin/query/compound/DisMaxTest.kt)
+ [Function Score](./src/test/kotlin/mbuhot/eskotlin/query/compound/FunctionScoreTest.kt)
+ [Boosting](./src/test/kotlin/mbuhot/eskotlin/query/compound/BoostingTest.kt)
+ [Indices](./src/test/kotlin/mbuhot/eskotlin/query/compound/IndicesTest.kt)

## Joining Queries - Done

+ [Nested](./src/test/kotlin/mbuhot/eskotlin/query/joining/NestedTest.kt)
+ [Has Child](./src/test/kotlin/mbuhot/eskotlin/query/joining/HasChildTest.kt)
+ [Has Parent](./src/test/kotlin/mbuhot/eskotlin/query/joining/HasParentTest.kt)

## Geo Queries - Not Yet Implemented

+ Geo Shape
+ Geo Bounding Box
+ Geo Distance Range
+ Geo Polygon
+ Geohash Cell

## Specialized Queries - Not Yet Implemented

+ More Like This
+ Template
+ Script

## Span Queries - Not Yet Implemented

+ Span Term
+ Span Multi Term
+ Span First
+ Span Near
+ Span Or
+ Span Not
+ Span Containing
+ Span Within


# License

MIT - See LICENSE file for full text.
