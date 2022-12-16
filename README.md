# Testing with JavaScript Lab

## Learning Goals

- Learn about testing in JavaScript
- Run JavaScript tests
- Learn how to read test results

## Introduction

Many of the labs you will complete in this program use tests. Passing the tests
verifies that the code you write behaves as desired and produces the expected
results. But writing tests is also a way to provide specifics about exactly how
the code should function. In a common development strategy known as [test-driven
development][tdd] (or TDD), programmers _first_ write the test for a specific
function of the code _then_ write the code to make the tests pass. TDD is
considered the most reliable methodology for delivering quality code.

[tdd]: https://en.wikipedia.org/wiki/Test-driven_development

What this means is that the results of running the tests will be an important
tool in figuring out how to get those tests passing. Code testing can feel like
an abstract concept at first, but it's worth starting to learn how they work.
When you're having difficulty passing a test, being able to read and understand
the test output — and the tests themselves — can be an invaluable skill.

## Getting Started

Recall that in the previous lesson, the tests were commented out. Be sure to fork
and clone this lab into your local environment so you have the version of the test
file that is not commented out.

Navigate into its directory in the terminal, then run `code .` to open the files
in Visual Studio Code. (If you are using a different text editor, the command
will be different.)

Open up `index.js` in your code editor. You are going to see mostly familiar
things:

```javascript
const name = "Joe";
const height = 74;
const message = `${name} is ${height} inches tall`;

module.exports = { name, height, message };
```

This should all look familiar except for that last line. You don't need to worry
about it for now — just know that line of code makes the variables available to
the test file.

Take a look at the `message` variable:

```js
const message = `${name} is ${height} inches tall`;
```

We can use `console.log` to take a look at the value of the `message` variable.
To do that, first type `console.log(message);` on the last line of `index.js`
and save the file. Next, navigate to the terminal, and type the following
command in the command line and hit enter (be sure you're still in the lab's
directory):

```console
$ node index.js
```

The `node` command _executes_ the code in whatever file you specify (in this
case, `index.js`). You should see `"Joe is 74 inches tall"` logged in the
terminal. If you don't, make sure you saved the `index.js` file.

> **Top Tip**: `console.log` is one of the debugging tools you can use as you're
> writing your code. Logging a variable and executing the code will allow you to
> verify that the value of the variable is what you're expecting.

In the line of code above, we are using _string interpolation_ to inject the
values of the `name` and `height` variables into the message. Recall that, for
this to work, you have to wrap the entire string in backticks and wrap the
variables themselves in `${}`. If you'd like a refresher, try leaving out the
`${}`s or switching to a different type of quotes and run your code again to see
what the value of `message` is. The backticks and the `${}` tell Javascript to
grab the _value_ inside the variable, not just that variable name.

Go ahead and delete the `console.log` from `index.js` before moving on.

### The Tests

We have our code, now let's take a look at the tests. They are located in the
`test` folder inside a file named `index-test.js`.

```javascript
const { name, height, message } = require("../index.js");

describe("what-is-a-test", () => {
  describe("Name", () => {
    it('returns "Susan"', () => {
      expect(name).toEqual("Susan");
    });
  });

  describe("Height", () => {
    it("is less than 40", () => {
      expect(height).toBeLessThan(40);
    });
  });

  describe("Message", () => {
    it("gives the name and height", () => {
      expect(message).toInclude(name);
      expect(message).toInclude(height);
    });
  });
});
```

**Important**: You should never need to make changes to test files unless a
lab's instructions specifically tell you to do so.

In the first line, we're enabling the tests to access the variables in
`index.js`. You don't need to worry about exactly how this works at this point —
just know that the `module.exports` and `require` keywords allow us to access
variables written in the `index.js` file from within the test file.

Next, note that the test code consists of three individual tests (each starting
with `describe`) nested inside a block for the tests as a whole (also starting
with `describe`).

The first grouping is testing our `name` variable:

```javascript
describe("Name", () => {
  it('returns "Susan"', () => {
    expect(name).toEqual("Susan");
  });
});
```

Take a look at the line that begins with `expect`. If we read it out loud, we
get "Expect `name` to equal Susan". That's exactly what it's saying! If we
continue down to the Height section you'll see this code:

```javascript
describe("Height", () => {
  it("is less than 40", () => {
    expect(height).toBeLessThan(40);
  });
});
```

Again, reading the line starting with `expect` out loud, we get "Expect `height`
to be less than 40." Again, this is just what the test is checking. Let's look
at the final one:

```javascript
describe("Message", () => {
  it("gives the name and height", () => {
    expect(message).toInclude(name);
    expect(message).toInclude(height);
  });
});
```

This one has two `expect` statements. If you read them out as English you'll
discover that the tests expect the value of `index.message` to include both
`index.name` and `index.height`.

OK great. Now that you understand what the tests are saying, it's time to run
them.

## Running Tests

To run the tests, make sure you're inside the lab's directory in the terminal,
then run `learn test`. Recall that this command first installs the lab's
dependencies, then shows the results of running the tests.

> What exactly do we mean by installing dependencies? Open the `package.json`
> file and scroll down to the bottom. You'll see a list of 'DevDependencies'.
> What's listed here are JavaScript _packages_: files or sets of files full of
> existing, reusable code. They are designed to be shared, allowing many
> developers to use the same code in their own projects. The packages you see
> listed in `package.json` make it possible to run the lab's tests. In order to
> use the packages, we have to install them. One of the things `learn test` does
> for us is run `npm install`, which is the command that installs the
> dependencies.

The next step is learning how to read the results that the tests give you.

## Reading Results of Tests

The first time you run `learn test`, you should see something that looks like
this:

```console
> js-functions-lab@0.1.0 test
> mocha --timeout 5000 -R mocha-multi --reporter-options spec=-,json=.results.json


  what-is-a-test
    Name
      1) returns "Susan"
    Height
      2) is less than 40
    Message
      ✓ gives the name and height


  1 passing (552ms)
  2 failing

  1) what-is-a-test
       Name
         returns "Susan":

      Error: Expected 'Joe' to equal 'Susan'
      + expected - actual

      -Joe
      +Susan

      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toEqual (node_modules/expect/lib/Expectation.js:81:30)
      at Context.<anonymous> (test/index-test.js:6:26)
      at processImmediate (internal/timers.js:461:21)

  2) what-is-a-test
       Height
         is less than 40:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/index-test.js:13:28)
      at processImmediate (internal/timers.js:461:21)

npm ERR! Test failed.  See above for more details.
```

> **Note**: If you also get an error that ends with "unexpected character (after
> ) at line 1, column 1 [parse.c:769] (Oj::ParseError)", go back to the
> `index.js` file and remove the `console.log` we added earlier, then run
> `learn test` again.

Let's break this down a bit. If you look about a third of the way down in the
output, you'll see a summary of how the tests went:

```txt
  1 passing (552ms)
  2 failing
```

Great! We've already got one test passing! Now let's see how we failed the other
two tests.

```txt
  1) what-is-a-test
       Name
         returns "Susan":

      Error: Expected 'Joe' to equal 'Susan'
      + expected - actual

      -Joe
      +Susan

      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toEqual (node_modules/expect/lib/Expectation.js:81:30)
      at Context.<anonymous> (test/index-test.js:6:26)
      at processImmediate (internal/timers.js:461:21)

  2) what-is-a-test
       Height
         is less than 40:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/index-test.js:13:28)
      at processImmediate (internal/timers.js:461:21)
```

While there is no hard and fast rule, and there will be exceptions, it is most
often best to address your test errors in order. So let's take a look at our
first error:

```txt
1) what-is-a-test
       Name
         returns "Susan":

      Error: Expected 'Joe' to equal 'Susan'
      + expected - actual

      -Joe
      +Susan

      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toEqual (node_modules/expect/lib/Expectation.js:81:30)
      at Context.<anonymous> (test/index-test.js:6:26)
      at processImmediate (internal/timers.js:456:21)
```

Here is the specific error:

```txt
      Error: Expected 'Joe' to equal 'Susan'
      + expected - actual

      -Joe
      +Susan
```

It tells us what the test is expecting (`Expected 'Joe' to equal 'Susan'`) and
then gives us details about the `expected` and `actual` values. This shows you
exactly how the value your code is returning (the `actual` value) differs from
what the test is looking for. Make sure you understand what this is telling you
— it will come in handy in later labs!

This error makes sense because we have the `name` variable set equal to "Joe" in
our `index.js` file. Let's change that line of code to set `name` equal to
"Susan" instead. Run the tests again by typing `learn test` in the terminal's
command line, and you should see that we are now passing 2 of the 3 tests!

```txt
  what-is-a-test
    Name
      ✓ returns "Susan"
    Height
      1) is less than 40
    Message
      ✓ gives the name and height


  2 passing (736ms)
  1 failing

  1) what-is-a-test
       Height
         is less than 40:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/index-test.js:13:28)
      at processImmediate (internal/timers.js:461:21)
```

Woot! You passed another one. Now go ahead and try to pass the remaining test on
your own.

### Common Errors

While you are solving the other tests you may come across a few errors. Let's go
over some common ones:

#### Variable Not Defined

```txt
ReferenceError: name is not defined
```

That one says that the `name` variable is not defined. That makes no sense! We
initialized the `name` variable in `index.js`! What that actually means is that
the test couldn't find the variable `name`. You'll get this error if the name of
one of your variables is different than the test is expecting. Check to make
sure you used the correct variable names and look carefully for typos.

#### Unexpected Identifier

```txt
/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/index.js:1
cnst name = "Susan";
     ^^^^

SyntaxError: Unexpected identifier
    at wrapSafe (internal/modules/cjs/loader.js:979:16)
    at Module._compile (internal/modules/cjs/loader.js:1027:27)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Module.require (internal/modules/cjs/loader.js:952:19)
    at require (internal/modules/cjs/helpers.js:88:18)
    at Object.<anonymous> (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/test/index-test.js:1:15)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Module.require (internal/modules/cjs/loader.js:952:19)
    at require (internal/modules/cjs/helpers.js:88:18)
    at /Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:311:36
    at Array.forEach (<anonymous>)
    at Mocha.loadFiles (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:308:14)
    at Mocha.run (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:849:10)
    at Object.exports.singleRun (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run-helpers.js:108:16)
    at exports.runMocha (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run-helpers.js:143:13)
    at Object.exports.handler (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run.js:305:3)
    at Object.runCommand (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/lib/command.js:242:26)
    at Object.parseArgs [as _parseArgs] (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/yargs.js:1104:24)
    at Object.parse (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/yargs.js:566:25)
    at Object.exports.main (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/cli.js:68:6)
    at Object.<anonymous> (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/bin/mocha:133:29)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
npm ERR! Test failed.  See above for more details.
```

Whoa! So many words that make no sense. Don't worry though. The most important
line is the `SyntaxError: Unexpected identifier` line. What that means is you
have some sort of typo or syntax mistake. It could be a HUGE variety of things
but usually, JS will try and give you a hint. This time it's pointing to the
`cnst name = "Susan"` line of code. Take a look and read _very carefully_:
`const` is misspelled. Whoops! Once we fix that everything will work.

One note on this type of error is that it is sort of a catch-all. Tons and tons
of problems end in that sort of error message. Whenever you see it, be sure to
read over your code with a fine-toothed comb... and you'll find the problem!

## Type Errors

On the second test, there is a chance you might see the following error:

```txt
1) what-is-a-test
       Height
         is less than 40:
     Error: The "actual" argument in expect(actual).toBeLessThan() must be a number
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/index-test.js:13:28)
      at processImmediate (internal/timers.js:456:21)
```

This error is slightly different than the last two. In this case, the test is
giving us a unique message because it recognizes a problem. If we look at this
test in `test/index-test.js`, we see this:

```js
describe("Height", () => {
  it("is less than 40", () => {
    expect(height).toBeLessThan(40);
  });
});
```

We can see that the word `"actual"` in this case is referring to the `height`
variable. The error message is telling us that `height` **must be a number**. If
you're seeing this, make sure that you have set the `height` variable to a
**number** that's less than 40 (e.g. `39`), not a **string** (`"39"`). The test
will interpret the value as a string due to the quotation marks wrapping it.

## Optional Mocha Configuration

In this lab, we only had three tests to pass, but as you continue through the
curriculum you will encounter labs with many more tests. You can imagine that
the test output could get very long, making it more difficult to focus in on how
to fix a particular error.

To help with this issue, there is some very simple setup you can put in place in
Mocha that will cause the tests to stop as soon as the first failing test is
encountered.

To implement this, open up the `package.json` file and find the test script. It
should look something like this:

```json
"test": "mocha --timeout 5000 -R mocha-multi --reporter-options spec=-,json=.results.json"
```

Add the `--bail` flag to the end of the line, inside the quotes:

```json
"test": "mocha --timeout 5000 -R mocha-multi --reporter-options spec=-,json=.results.json --bail"
```

That's it!

## Conclusion

Once you've got all your tests passing, you're ready to move on.
Congratulations! You've solved your first JavaScript tests!
