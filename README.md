# Testing with JavaScript Lab

## Learning Goals

- Running tests
- Reading test results

## Running Tests

You understand how to read tests; now it's time to run the tests.

If you haven't already, fork and clone this lesson into your local environment.
Navigate into its directory in the terminal, then run `code .` to open the files
in Visual Studio Code. (If you are using a different text editor, the command
will be different.) Finally, run `npm install` to install the lab's
dependencies.

> What exactly do we mean by installing dependencies? Open the `package.json`
> file and scroll down to the bottom. You'll see a list of 'DevDependencies'.
> What's listed here are JavaScript _packages_: files or sets of files full of
> existing, reusable code. They are designed to be shared, allowing many
> developers to use the same code in their own projects. The packages you see
> listed in `package.json` make it possible to run the lab's tests. In order to
> use the packages, we have to install them; `npm install` does that for us.

If you take a look at `index.js` and `index-test.js`, you should see the same
code as in the previous lesson. The only difference is that the test code in
`index-test.js` is no longer commented out.

**Important**: You should never need to make changes to test files unless a
lab's instructions specifically tell you to do so.

To run the tests, run `npm test` in the terminal. That's it!

The next step is learning how to read the results that the tests give you.

## Reading Results of Tests

The first time you run `npm test`, you should see something that looks like
this:

```txt
> js-functions-lab@0.1.0 test /Users/mbenton/Desktop/curriculum-team/js-what-is-a-test-lab
> mocha -R mocha-multi --reporter-options spec=-,json=.results.json

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

Let's break this down a bit. If you look about a third of the way down in the
output, you'll see a summary of how the tests went:

```txt
  1 passing (552ms)
  2 failing
```

Great! We've already got one test passing! Now let's see how we failed the
other two tests.

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
"Susan" instead. Run the tests again by typing `npm test` in the terminal's
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
/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/index.js:1
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
    at Object.<anonymous> (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/test/index-test.js:1:15)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Module.require (internal/modules/cjs/loader.js:952:19)
    at require (internal/modules/cjs/helpers.js:88:18)
    at /Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:311:36
    at Array.forEach (<anonymous>)
    at Mocha.loadFiles (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:308:14)
    at Mocha.run (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/mocha.js:849:10)
    at Object.exports.singleRun (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run-helpers.js:108:16)
    at exports.runMocha (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run-helpers.js:143:13)
    at Object.exports.handler (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/run.js:305:3)
    at Object.runCommand (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/lib/command.js:242:26)
    at Object.parseArgs [as _parseArgs] (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/yargs.js:1104:24)
    at Object.parse (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/yargs/yargs.js:566:25)
    at Object.exports.main (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/lib/cli/cli.js:68:6)
    at Object.<anonymous> (/Users/lizburton_fs/Development/code/curriculum/flip/pac3/phase-0-pac-3-what-is-a-test-lab/node_modules/mocha/bin/mocha:133:29)
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

We can see that the word `"actual"` in this case is referring to the
`height` variable. The error message is telling us that `height`
**must be a number**. If you're seeing this, make sure that you have set the
`height` variable to a **number** that's less than 40 (e.g. `39`), not a
**string** (`"39"`). The test will interpret the value as a string due to the
quotation marks wrapping it.

## Saving Your Work Remotely

Currently, the work you've done on this assignment is only on your local
machine. To preserve work on your GitHub fork, you will need to stage the
changes you've made, commit them, and push the commit up to GitHub. Use
the following commands to do this:

```console
$ git add .
$ git commit -m "Completed assignment"
$ git push
```

If you visit your fork on GitHub, you should now see that _you've_ made the most
recent commit, and your solution will be present in the files.

## Conclusion

Once you've got all your tests passing, you're ready to move on.
Congratulations! You've solved your first JavaScript tests!
