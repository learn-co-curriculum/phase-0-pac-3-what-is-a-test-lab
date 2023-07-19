# Testing with JavaScript Lab

## Learning Goals

- Running tests
- Reading test results

## Running Tests

You understand how to read tests; now it's time to run the tests.

Recall that in the previous lesson, the tests were commented out. Be sure to **fork
and clone** this lab into your local environment so you have the version of the test
file that is not commented out. (Return to the previous lesson if you need a reminder
of how to do this.)

Navigate into its directory in the terminal, then run `code .` to open the files
in Visual Studio Code. (If you are using a different text editor, the command
will be different.) Finally, run `npm install` to install the lab's
dependencies.

> What exactly do we mean by installing dependencies? Open the package.json file
> and scroll down to the bottom. You'll see a list of 'DevDependencies'. What's
> listed here are JavaScript packages: files or sets of files full of existing,
> reusable code. They are designed to be shared, allowing many developers to use
> the same code in their own projects. The packages you see listed in
> package.json make it possible to run the lab's tests. In order to use the
> packages, we have to install them; npm install does that for us.

If you take a look at `index.js` and `indexTest.js`, you should see the same
code as in the previous lesson. The only difference is that the test code in
`indexTest.js` is no longer commented out.

**Important**: You should never need to make changes to test files unless a
lab's instructions specifically tell you to do so.

To run the tests, run `npm test` in the terminal. That's it!

The next step is learning how to read the results that the tests give you.

## Reading Results of Tests

The first time you run `npm test`, you should see something that looks like
this:

```console
> js-functions-lab@0.1.0 test
> mocha --timeout 5000 -R mocha-multi --reporter-options spec=-,json=.results.json


  what-is-a-test
    Name
      1) returns "Susan"
    Height
      2) is less than 40 and greater than 0
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
      at Context.<anonymous> (test/indexTest.js:6:26)
      at processImmediate (internal/timers.js:461:21)

  2) what-is-a-test
       Height
         is less than 40 and greater than 0:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/indexTest.js:13:28)
      at processImmediate (internal/timers.js:461:21)

npm ERR! Test failed.  See above for more details.
```

> **Note**: If you also get an error that ends with "unexpected character (after
> ) at line 1, column 1 [parse.c:769] (Oj::ParseError)", make sure you've cloned
> down the files for this lab, and are not running the tests on the files from
> the previous lesson.

Let's break this down a bit. If you look about a third of the way down in the
output, you'll see a summary of how the tests went:

```console
  1 passing (552ms)
  2 failing
```

Great! We've already got one test passing! Now let's see how we failed the other
two tests.

```console
  1) what-is-a-test
       Name
         returns "Susan":

      Error: Expected 'Joe' to equal 'Susan'
      + expected - actual

      -Joe
      +Susan

      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toEqual (node_modules/expect/lib/Expectation.js:81:30)
      at Context.<anonymous> (test/indexTest.js:6:26)
      at processImmediate (internal/timers.js:461:21)

  2) what-is-a-test
       Height
         is less than 40 and greater than 0:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/indexTest.js:13:28)
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
      at Context.<anonymous> (test/indexTest.js:6:26)
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
command line (don't forget to save the file first!), and you should see that
we are now passing 2 of the 3 tests!

```txt
  what-is-a-test
    Name
      ✓ returns "Susan"
    Height
      1) is less than 40 and greater than 0
    Message
      ✓ gives the name and height


  2 passing (736ms)
  1 failing

  1) what-is-a-test
       Height
         is less than 40 and greater than 0:
     Error: Expected 74 to be less than 40
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/indexTest.js:13:28)
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
    at Object.<anonymous> (/Users/lizburton_fs/Development/code/phase-0-pac-3-what-is-a-test-lab/test/indexTest.js:1:15)
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
         is less than 40 and greater than 0:
     Error: The "actual" argument in expect(actual).toBeLessThan() must be a number
      at assert (node_modules/expect/lib/assert.js:29:9)
      at Expectation.toBeLessThan (node_modules/expect/lib/Expectation.js:156:28)
      at Context.<anonymous> (test/indexTest.js:13:28)
      at processImmediate (internal/timers.js:456:21)
```

This error is slightly different than the last two. In this case, the test is
giving us a unique message because it recognizes a problem. If we look at this
test in `test/indexTest.js`, we see this:

```js
describe("Height", () => {
    it("is less than 40 and greater than 0", () => {
      expect(height).toBeMoreThan(0)
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

## Submitting Your Work to Canvas

Once you've got all the tests passing, it's time to push your completed code up
to GitHub and submit it to Canvas using CodeGrade. We'll do a quick review of
how to do that below, but you may want to review the full process in the
[Complete Your First Software Engineering Assignment][complete-assignment]
lesson. You'll be going through this process for every lab you do in this
program!

[complete-assignment]: https://github.com/learn-co-curriculum/phase-0-completing-assignments-codegrade

Let's review the process. First, you need to "stage" your changes using the `git
add` command:

```console
$ git add index.js
```

or

```console
$ git add .
```

Recall that the `.` shortcut will stage **all** files that have changes. In this
case there's only one so either command will work.

Next, you need to "commit" your changes, which basically saves a record of the
changes you've made. Don't forget to use the `-m` flag and include a commit
message! Use the message shown below or choose your own:

```console
$ git commit -m "complete lab"
```

Finally, push your changes up to your GitHub account (your fork of this lab):

```console
$ git push
```

If you go back to your repo in GitHub and refresh the page, you should now see a
new commit with your commit message.

The final step is to submit your work to Canvas:

1. Scroll to the bottom of this lesson page in Canvas and click the button
   labeled "Load Review: Variables Lab in a new window".
2. In the CodeGrade window that opens, click "Create Submission". You should now
   see a list of your repositories.
3. Find the repo for this lab and click Connect.
4. When you get the message that your repo has been connected, click on the
   embedded link, then the "AutoTest" tab to watch your progress. Once the tests
   have finished running, you should see the green checkmark in the "Pass"
   column, indicating that you've successfully completed the lab.

![CodeGrade window showing tests have all been passed](https://curriculum-content.s3.amazonaws.com/phase-0/completing-assignments-codegrade/codegrade-tests-passing.png)

### Note about the `git push` command

You may recall from the [Complete Your First Software Engineering
Assignment][complete-assignment] lesson that a different form of the `git push`
command was used:

[complete-assignment]: https://github.com/learn-co-curriculum/phase-0-completing-assignments-codegrade

```console
$ git push <remote> <branch>
```

where `remote` is the "alias" for the repo's url on GitHub, and `branch` is the
repo's default branch (generally `main` for newer repos and `master` for older
ones). For this lab, therefore, the full command would be:

```console
$ git push origin master
```

This command tells git to push the code in the `master` branch of the local repo
to the `master` branch of the remote repo identified by the `origin` alias.

So why didn't you need to run that command?

When you use the `git clone` command to clone down a repo from GitHub, git
automatically assigns the "origin" alias to the url you clone from, and uses
the default branch for that repo.

As you work through the labs in this program, you should always:

1. **fork** the lab's repo to your GitHub account, and
2. **clone** that fork down to your local machine.

As long as you always fork before you clone, it should be safe to run `git
push` without specifying the remote and branch.

If you want to verify that you're pushing to the right repo, you can use the
`git remote` command and include the `-v` flag:

```console
$ git remote -v
origin	git@github.com:your-github-username/phase-0-pac-3-what-is-a-test-lab.git (fetch)
origin	git@github.com:your-github-username/phase-0-pac-3-what-is-a-test-lab.git (push)
```

Here you can see that the `origin` alias points to your fork of the repo, so
it's safe to run the shorter command, `git push`.

## Conclusion

Now that you've gotten all your tests passing and submitted your work (and
learned a bit more about `git push`), you're ready to move on. Congratulations!
You've solved your first JavaScript tests!
