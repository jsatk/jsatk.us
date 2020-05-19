+++
draft = false
date = 2016-10-19T00:00:00-00:00
title = "Testing Promise Rejections with Chai-As-Promised"
description = ""
slug = ""
tags = []
categories = []
externalLink = ""
series = []
+++

I was recently working on a new npm module for work which is a promise-based API. Testing promises in javascript is very do-able, but also very tricky. You can test promises and their results in many various ways. Unfortunately the JS community seems still somewhat divided on how to properly test promises.
The biggest problem I faced was that I needed to mock a third-party API. I avoid stubbing as much as humanly possible, but it is often necessary when testing third party code. I'm a strong believer in that your unit tests should run in an isolated (read: offline) environment. I use [Sinon](https://sinonjs.org/) when I need to mock out dependency functionality.

I knew about the lovely [chai-as-promised](https://github.com/domenic/chai-as-promised), but I stubbornly resisted adding another dependency to my `package.json`. After creating a [Rube Goldberg machine](https://en.wikipedia.org/wiki/Rube_Goldberg_machine) to test my promises, I gave in and added chai-as-promised to my module. And for a brief moment I was happy.

---

So I write a test and watch it fail (like the good TDD developer I am). Here's a simplified psuedo example of what I was working on below.

```js
import thirdPartyClient from 'some-third-party-client'
import myApi from '../../src/my-api'

describe('myApi', () => {
  describe('Error handling', () => {
    const myRejection = {
      errorCode: 404,
      errorMessage: 'You broke everything',
      errorUrl: 'http://coolapp.api.docs/error/404/'
    }

    beforeEach(() => {
      sinon.stub(thirdPartyClient.prototype, 'dingus')
         .returns(Promise.reject(myRejection)
    })

    afterEach(() => thirdPartyClient.prototype.dingus.restore())

    it('should be rejected with the correct error object', () => {
      const expected = myRejection

      return myApi.someMethod()
        .then(() => {
          const actual = thirdPartyClient.prototype.dingus
          return expect(actual).to.be.rejectedWith(expected)
        })
    })
  })
})
```

I watch it fail.

I write the code.

I watch it pass.

Success, right?

Something didn't feel right. So I decided I wanted to see this test fail again. I added a `not` to the `expect` statement on line 21 so that it read `return expect(actual).not.to.be.rejectedWith(expected)`. Unfortunately this also passed.

I realized I wasn't doing a deep comparison on the rejection. It was simply testing to see if it was indeed an object and passing if it was.
So how do I test if a promise is rejected and also deeply test the rejection?

---

My coworker [Pete Hodgson](https://medium.com/u/d41b471adec6?source=post_page-----c65c7c33f329----------------------) mentioned that the latest version of chai-as-promised supports some very declarative syntax and gave me this example: `expect(foo).to.be.rejected.and.eventually.have.property('quxx')`. I was overjoyed.

Now I had a way to both test if something was rejected but also what the rejection actually was.

I updated the above code to now read:

```js
import thirdPartyClient from 'some-third-party-client'
import myApi from '../../src/my-api'

describe('myApi', () => {
  describe('Error handling', () => {
    const myRejection = {
      errorCode: 404,
      errorMessage: 'You broke everything',
      errorUrl: 'http://coolapp.api.docs/error/404/'
    }

    beforeEach(() => {
      sinon.stub(thirdPartyClient.prototype, 'dingus')
         .returns(Promise.reject(myRejection)
    })

    afterEach(() => thirdPartyClient.prototype.dingus.restore())

    it('should be rejected with the correct error object', () => {
      const expected = myRejection

      return myApi.someMethod()
        .then(() => {
          const actual = thirdPartyClient.prototype.dingus
          return expect(actual).to.be.rejected
            .and.to.eventually.deep.equal(expected)
        })
    })
  })
})
```

This is not only much easier to read but allows for a deep equals comparison on the rejection.

---

In summary, there is now a nice and declarative way to test promises and what they actually return using chai-as-promised. I avoid adding unnecessary dependencies, but I found the benefits here far outweighed the cost of having another dependency.

Thanks for reading and I hope this was helpful!
