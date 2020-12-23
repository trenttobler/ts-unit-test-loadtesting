# Typescript Unit Testing / framework load test

The intent of this package is to explore performance of various unit test frameworks available for writing typescript code using TDD, ["Red Green Refactor" methodology](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html).

Basic expectations of an acceptable test framework supporting this methodology: a "all tests pass", or indication of at least one test failure, within 2-3 seconds... ideally, less than 1 second.

I created this project after a frustratingly amount of time trying to get an internal client react app running at acceptable iteration cycle times.  Jest was taking 60+ seconds to complete a test iteration, with only ~500 tests.  Some of this was environmental, and test methodology, but as I tried to find the right set of options, environment, and test strategy changes to make this fast, I could only get the run down to 20 seconds.  This is still an order of magnitude higher and not acceptable for Red Green Refactor methodology.

In exploring typescript on my own, I had already implemented several suggestions and ideas around mocha, chai, and nyc coverage testing framework.  Under this system on my toy project, I easily attained the desired iteration cycle time (<20ms per test iteration for 50-100 tests, and 1-2 seconds for code coverage report).

I already had a vision in mind, based on my experience in this toy project: make TDD test reports part of your "dashboard" - the only trigger to see updated status of tests is to just ensure all changes are saved to disk.

To verify my assumptions that something in the production react app "jest" environment itself was cause for slow execution, I decided to try and create a comparison by which concrete examples can be evaluated and optimized.

With a sequence of 577 test files (approaching the number of test files in the production project), I chose to do nothing more than a simple const value assertion, one written in the style noted in the create-react-app template, and one using mocha and chai syntax.  If this cannot be made fast, then there is no chance once other layers are stacked that this iteration vision will be achievable.

The following documents the initial set of results and timings I got.  The Test procedure is as follows:

* Ensure docker is running (linux mode).
* start wsl command line
* git clone the source code repository
* launch visual studio code, with [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote) extension pack installed.
* using the extension, launch docker development instance, mounting the wsl folder for one subproject.
* Using visual studio code "terminal" command line (bash) to provide access inside the container, execute the steps corresponding to the specific folder - see below in test results.

## create-react-app / Jest results
```
> npm install
[...]

> time npm test -- --watchAll false

[...]

Test Suites: 244 passed, 244 total
Tests:       568 passed, 568 total
Snapshots:   0 total
Time:        15.765 s
Ran all test suites.

real    0m16.977s
user    1m44.347s
sys     0m14.007s

> time npm test -- --watchAll false

[...]

Test Suites: 244 passed, 244 total
Tests:       568 passed, 568 total
Snapshots:   0 total
Time:        18.166 s
Ran all test suites.

real    0m19.486s
user    1m59.289s
sys     0m17.362s
```

## Mocha+Chai results
```
> npm install
[...]

> time npm test

  27135 passing (2s)

real    0m4.296s
user    0m4.590s
sys     0m0.865s
node ➜ /workspaces/ts-unit-test-loadtesting/mocha-tdd (master ✗) $ 

> time npm test

  27135 passing (3s)

real    0m4.599s
user    0m4.820s
sys     0m1.039s
node ➜ /workspaces/ts-unit-test-loadtesting/mocha-tdd (master ✗) $ 

```

## Testing Environment

```
Intel Core i7-7820HQ @ 2.9GHz x2, 32GB RAM

Windows 10 Enterprise : 1909 build 18363.1256

WSL 2 - Ubuntu-20.04
  > uname -a
  4.19.128-microsoft-standard #1 SMP Tue Jun 23 12:58:10 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

Docker Desktop 2.4
Docker version 19.03.13, build 4484c46d9d

Visual Studio Code 1.52.1
```

all other dependencies are contained within the docker development environment.
