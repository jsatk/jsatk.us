---
title: "Working With Thrift Unions in Scala"
date: 2021-03-21T15:53:15-07:00
draft: false
---

Lately I've been working a lot with Unions in Scala and have found it to be mildly confusing.  It's easy to get tripped up so I wanted to write down a guide of sorts on how to work with them, particularly in Scala.

## A brief primer

First lets go over what we're dealing with here when we're dealing with an IDL
in Scala.  IDL stands for [Interactive Data Language
(IDL)](https://en.wikipedia.org/wiki/IDL_(programming_language)).  The codebase I primarily work in in my day-to-day we write our IDLs in [Thrift](https://thrift.apache.org).  And we use [Scrooge](https://github.com/twitter/scrooge) to compile Thrift into Scala.  So when you're in a Scala repo and need to use IDL models what you're dealing with is Scala code that was autogenerated via Scrooge from Thrift.  Still following me?  If not that's okay.  The TL;DR is we write models in Thift and a tool called Scrooge turns that into Scala.  This is why if you've ever used a "Go To Definition" feature when working with a model in Scala the code it takes you to is difficult to read and doesn't look like it was written by a human.  That's because it wasn't.

## An example

### Accessing a type we already have

Lets say we have the following Thrift code.

```thrift
// filename: datalogging.thrift
// Old API
struct RecordA {
    1: required RequestA request
    2: optional ResponseA response
}

// New API
struct RecordB {
    1: required RequestB request
    2: optional ResponseB response
}

union Record {
    1: RecordA v1
    1: RecordB v2
}

struct DataRecord {
    1: optional Record record
}
```

And the following Scala code.

```scala
import com.foo.datalogging.{ Record, DataRecord }
import com.foo.datalogging.DataRecord.{ UnknownUnionField, V1, V2 }

class Foo {
  def apply(record: DataRecord): Option[Record] = ???
}
```

How do we figure out what Union type we're dealing with?  In our example Thrift we have two subtypes.  We need to figure out what we're dealing with so the compiler can know.

Here's how we'd do that.

```scala
import com.foo.datalogging.{ Record, DataRecord }
import com.foo.datalogging.DataRecord.{ UnknownUnionField, V1, V2 }

class Foo {
  def apply(record: DataRecord): Option[Record] =
    // First we need to `map` because `record` is an [[Option]].
    record.prophecyAO.flatMap(p =>
      // Here we cannot do `p.v1.request`.   Again, we need to figure out what type we're dealing with.
      p match {
        case V1(v1) => Some(v1) // 👈 Inside this case we now know that `v1` is a subtype [[Record]] and can treat it as such.
                                // We can now access the `response` on `v1`.
        case V2(v2) => Some(v2) // 👈 Inside this case we now know that `v2` is a subtype [[Record]] and can treat it as such.
                                // We can now access the `response` on `v2`.
        case UnknownUnionField(_) => None // Safety first!
      }
    )
}
```

We can even drop the `match` here.

```scala
import com.foo.datalogging.{ Record, RecommenderRecord }
import com.foo.datalogging.DataRecord.{ UnknownUnionField, V1, V2 }

class Foo {
  def apply(record: RecommenderRecord): Option[Record] =
    record.prophecyAO.flatMap(
      case V1(v1) => Some(v1)
      case V2(v2) => Some(v2)
      case UnknownUnionField(_) => None
    )
}
```

One of the things I found most confusing here is the uppercase `V1`.  When dealing with a Union type Scrooge starts the property name with a capital because you'll only ever access it as a case class.  If this sounds confusing just note that with Union types each subtype will be accessible via `UnionType.SubType`; not `UnionType.subType`.

Another point of confusion is that the code Scrooge generates often doesn't 1-to-1 match up with what you're seeing in Thrift.  For example in the above code if you were to inspect the type of `v1` your editor would tell you that it's of type `RecordAliases.V1Alias` .  The `Alias` stuff is normal and – again – generated by Scrooge.

### Creating a type when we have all the parts to make that type

What about a situation where we have a request of type `RequestB` and a response of type `ResponseB` and we want to take this request and response and create a `DataRecord`?  In our (admittedly trivial) example Thrift we have all the parts we need.

```scala
import com.foo.datalogging.{ RequestB, ResponseB }
import com.foo.datalogging.{ Record, DataRecord }
import com.foo.datalogging.DataRecord.V2

class Foo {
  def apply(request: RequestB, response: ResponseB): RecommenderRecord =
    DataRecord(
      Some( // Because we're dealing with an [[Option]].
        V2( // The name of the property on [[Record]] we're setting.
          Record( // The type that [[V2]] needs to be.
            request, // We already know this is a [[RequestB]].
            Some(response) // We already know this is a [[ResponseB]], and the model requires it to be a [[Option]].
          )
        )
      )
    )
}
```

Personally, I've found the this part a lot more challenging than the first part.  Figuring out what type we're dealing with is typically a simple match statement and most editors will literally auto-fill-in your `case` statements for you via exhaustive match.  Creating a Union when you have all the parts for one has stumped me a few times.  In this example the `V2` bit always trips me up.

### Modifying a union type with lenses

Sometimes you need to modify a Union type and you want to use a lense (you should be using lenses) rather than convoluted `.copy` statements.  In this example we'll be using [Quicklens](https://github.com/softwaremill/quicklens).  The key thing you're looking for here is `.when[Type]`.  Other lense libraries have the same or similar methods and the API shouldn't look much (if any) different.

```scala
import com.foo.datalogging.{ Record, DataRecord }
import com.foo.datalogging.Record.{ UnknownUnionField, V1 }
import com.softwaremill.quicklens._

class Foo {
  def apply(record: DataRecord): Option[Record] =
    record.modify(_.prophecyAO.each.when[V1].v1.response.each).setTo(???)
}
```

## Conclusion

Scrooge has really great docs on [Unions](https://twitter.github.io/scrooge/GeneratedCodeUsage.html) here if you want a more thorough explanation.  However I found these to be a tad confusing and didn't spell out the real world examples I was encountering.

Please [email me](mailto:jesse@jsatk.us) any comments, thoughts, or corrections.

I hope this was helpful.  Thanks for reading.