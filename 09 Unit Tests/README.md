![Unit Tests](https://drive.google.com/uc?export=view&id=1aBPUTdEOVZFDNy9uIkQUzCLvgwGai8XZ)

### What is a unit test?

Verify that _known_, _fixed_ input produces a _known_, _fixed_ output.

1. Never generate random input. Always use fixed value.  
2. Don't use named constants from the model code. They may wrong or change. Prefer literal string and numbers.  
3. Don't access the network and preferably not the file system.  
4. Control time, the speed of light, or the gravitational constant of the universe.

What if we don't know the correct output?  
1. If it's a deterministic answer, write a [characterization test](https://www.artima.com/weblogs/viewpost.jsp?thread=198296).  
2. If the problem is fuzzy or not perfectly defined, test a similar problem with less fuzzy answer.

### Why do we write a unit test?

1. We want our code work.  
 1.a. We want it to keep working.  
2. We want to develop faster with more confidence and fewer regression.  
 2.a. We want to see the mistakes earlier.  
 2.b. We want to understand about our code more.  
3. We always make mistakes.

### Fundamental Principle of Unit Tests

##### Write your test first!

It's about software development. We can create better API because we start with the user, not the used.  
Test first hides implementation and avoid exposing internal implementation details. It avoids brittle, tightly coupled tests.

##### Eliminate everything that makes input or output unclear or contingent

##### Independent

1. Tests can (and do) run in any order.  
2. Tests can (and do) run in parallel in multiple threads.  
3. Tests should not interfere with each other.

##### Don't share data between tests

1. Don't use non-constant static field in your tests.  
2. Be wary of global state in the model code under test.  
3. Share setup in a fixture, not the same method.  
4. Don't use synchronization, [semaphores](https://www.baeldung.com/cs/semaphore) or special data structures.

##### We can have many test classes per model class

Every test that needs a slightly different setup can go into separate test class.

##### Fast

Run the test as fast as possible. A single test should run in a second or less. A complete suite should run in a minute or less.  
Separate larger tests into additional suites. Run the slowest tests last.

##### Passing test should produce no output

##### Failing test should produce clear output

Failing tests should give clear, unambiguous error message.  
Rotate the test data. This makes it much easier to see immediately which test is failing and why.

##### Avoid flakiness

Flakiness is when our tests sometimes pass, sometimes fail.  
The source can come from:  
1. Time dependence.  
2. Network availability.  
3. Explicit randomness.  
4. Multithreading.

###### System Skew

Special kind of flakiness. Our test pass all the time in local environment but always fail when run in others.  
The source can come from:  
1. Assumptions about underlying operating system.  
2. Undefined behavior.  
 2.a. Floating point roundoff.  
 2.b. Integer width.  
 2.c. Default character set.  
 2.d. etc.

##### Write failing test before debugging

If the test passes, the bug isn't what you think it is.

##### Before refactoring, break the code

Break the code before we refactor it to see if the test fail. The check our code coverage. If necessary, write additional tests before doing unsafe refactorings.

### The Three Laws of TDD

1. You may not write production code until you have written a failing unit test.  
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.  
3. You may not write more production code than is sufficient to pass the currently failing test.

### Clean Test

The tests must change as the production code evolves. The dirtier the tests, the harder they are to change. The more tangled the test code, the more likely it is that you will spend more time cramming new tests into the suite than it takes to write the new production code.

##### For us, clean test are all about:

###### Readability

What makes tests readable? The same thing that makes all code readable: clarity, simplicity, and density of expression.

```java
public void testGetPageHieratchyAsXml() throws Exception
{
	crawler.addPage(root, PathParser.parse("PageOne"));
	crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
	crawler.addPage(root, PathParser.parse("PageTwo"));

	request.setResource("root");
	request.addInput("type", "pages");
	Responder responder = new SerializedPageResponder();
	SimpleResponse response =
		(SimpleResponse) responder.makeResponse(
				new FitNesseContext(root), request);
	String xml = response.getContent();

	assertEquals("text/xml", response.getContentType());
	assertSubString("<name>PageOne</name>", xml);
	assertSubString("<name>PageTwo</name>", xml);
	assertSubString("<name>ChildOne</name>", xml);
}
```  
vs  
```java
public void testGetPageHierarchyAsXml() throws Exception {
	makePages("PageOne", "PageOne.ChildOne", "PageTwo");

	submitRequest("root", "type:pages");

	assertResponseIsXML();
	assertResponseContains(
			"<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
	);
}
```

###### Trustworthy

Make your test trustworthy:  
1. Test the right thing.  
2. Don't remove existing test except test case already invalid (requirement  changes).  
3. Assuring code coverage.  
4. Avoid test logic.  
5. Make it easy to run.

###### Maintainable

Creating maintainable tests  
1. Avoid testing private/protected members (test only public).  
2. Re-use test code (create, manipulate, assert).  
3. Enforce test isolation.  
4. Avoid multiple asserts.

##### How we create a clean tests?

Follow these rules  
1. Fast. Tests should be fast. They should run quickly.  
2. Independent. Tests should not depend on each other.  
3. Repeatable Tests should be repeatable in any environment.  
4. Self-Validating The tests should have a boolean output.  
5. Timely The tests need to be written in a timely fashion. Unit tests should be written just before the production code that makes them pass.

[Effective Unit Testing by Eliotte Rusty Harold](https://www.youtube.com/watch?v=fr1E9aVnBxw)