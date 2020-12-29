---
currentMenu: code-coverage
---

## Code Coverage

Kahlan comes with some built-in code coverage exporters (Istanbul, clover, lcov and throuhg terminal) which can be using in services like Coveralls, Scrutinizer or Code Climate. However Kahlan can also be customized through the [kahlan-config.php file](config-file.md) to setup another kind of export.

By default Kahlan will use the `Xdebug` driver to collect the code coverage. However by using `phpdbg` you won't need `Xdebug` at all. Kahlan will be able to collect the code coverage through `phpdbg` which has a built-in code coverage driver. So if your PHP install is including `Xdebug` you'll be able to replace `phpdbg -qrr ./bin/kahlan` by directly `./bin/kahlan` in the following examples.

![warning](assets/warning.png) while a way faster than `Xdebug`, the `phpdbg` built-in code coverage driver might be not as accurate as `Xdebug`.

### Enable the coverage with the `--coverage` option

The **`--coverage=<integer>`** option will generates some code coverage summary depending on the passed integer.

* 0: no coverage
* 1: code coverage summary of the whole project
* 2: code coverage summary detailed by namespaces
* 3: code coverage summary detailed by classes
* 4: code coverage summary detailed by methods

However sometimes it's interesting to see in details all covered/uncovered lines of a specific part of the code. To achieve this, you can pass a string to the `--coverage` option.

With **`--coverage=<string>`**, some detailed code coverage according to the specified namespace, class or method scope will be generated.

As an example, the following command will give the detailed code coverage of the `Kahlan::run()` method:

```bash
phpdbg -qrr ./bin/kahlan --coverage="Kahlan\Cli\Kahlan::run()"
```

![warning](assets/warning.png) don't forget to correctly set the `--src` option if your source directory is not `src/`.

**Note:**
All available namespaces, classes or methods definitions can be extracted from a simple `--coverage=4` code coverage summary.

```bash
phpdbg -qrr ./bin/kahlan --coverage
```

### Export the coverage

With **`--istanbul=<file>`** the code coverage will be exported in a file compatible with the istanbul Javascript library.

First install the library if not installed:
```bash
npm install -g nyc
```

Than you can use `nyc` to create an HTML Code Coverage report like so:

```bash
mkdir .nyc_output
phpdbg -qrr ./bin/kahlan --istanbul=.nyc_output/coverage.json
nyc report --reporter=html --extension=".php"
google-chrome coverage/index.html
```

You'll find the HTML Code Coverage report in `coverage/lcov-report/index.html`.

With **`--lcov=<file>`** the code coverage will be exported in a lcov file.

You can use `genhtml` (from the [lcov](http://ltp.sourceforge.net/coverage/lcov.php) package) to create an HTML Code Coverage report for example:

First install the command if not installed:
```bash
sudo apt-get install lcov
```

```bash
mkdir lcov
 ./bin/kahlan --lcov="lcov/coverage.info"
cd lcov
genhtml coverage.info
```

With **`--clover=<file>`** the code coverage will be exported in a clover file.

### Parallel testing

With the advent of Docker and its toolset, it's becoming easier to parallelize tests execution by using VMs to speed up a CI process.

To split your specs with Kahlan you can choose to split your test suite in different directories to be able to run them independently.

```php
phpdbg -qrr ./bin/kahlan --spec=spec/subset1
phpdbg -qrr ./bin/kahlan --spec=spec/subset2
phpdbg -qrr ./bin/kahlan --spec=spec/subset3
```

The second approach is to let Kahlan to automatically do the splitting by using the `--part=X/Y` option like so:

```php
phpdbg -qrr ./bin/kahlan --part=1/3
phpdbg -qrr ./bin/kahlan --part=2/3
phpdbg -qrr ./bin/kahlan --part=3/3
```

**X** represent the number of the split_to_run
**Y** represent The total number of splits you want

You can then parallelize your tests with the tooling of you choice like Github Actions for example:

```
name: Test

on: [pull_request]

jobs:

  kahlan-part1:
    name: "API: kahlan(1)"
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: "Running Kahlan(1)"
      run: ./ci/test-script1.sh

  api-kahlan-part2:
    name: "API: kahlan(2)"
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: "Running Kahlan(2)"
      run: ./ci/test-script2.sh

  api-kahlan-part3:
    name: "API: kahlan(3)"
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: "Running Kahlan(3)"
      run: ./ci/test-script3.sh
```

If you are lucky enough the code review/coverage tool you are using for your project support multiple uploads of code coverage (like [codecov.io](https://docs.codecov.io/docs/merging-reports) or [coveralls.io](https://docs.coveralls.io/parallel-build-webhook) for example). Otherwise you'll need to combining all coverage reports into one individual coverage report that can be submitted. Some tool like [codeclimate](https://docs.codeclimate.com/docs/configuring-test-coverage#section-parallel-tests) provides some tooling to make it easier, otherwise you'll need to do the merging on your own.
