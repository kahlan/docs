---
currentMenu: pro-tips
---

## Pro Tips

### Use the `--ff` option (fast fail)

`--ff` is the fast fail option. If used, the test suite will be stopped as soon as a failing test occurs. You can also specify a number of "allowed" fails before stopping the process. For example:

```
./bin/kahlan --ff=3
```

will stop the process as soon as 3 specs `it` failed.

### Use the focused mode

When writing your tests sometimes you want to **only execute** the test(s) you are working on. For this, you can prefix your spec with an "f" like in the following example:

```php
describe("test focused mode", function() {
    it("will be ignored", function() {
    });

    it("will be ignored", function() {
    });

    fit("will be run", function() {
    });
});
```

If you want to run a subset instead of a single test you can use `fdescribe` or `fcontext` instead.

**Tip:** combined with `--coverage=<string>` this is a powerful combo to see exactly what part of the code is covered for a subset of specs only.

### Comment out a spec

To comment out a spec, you can use the `x` prefix i.e. `xdescribe`, `xcontext` or `xit`.

### Skip a spec

To skip a spec you should use a `skipIf()` function inside of it. This function takes a boolean, that mean you can provide a conditions to skip this spec up. In example:

```php
it("should not run on weekends", function() {
    skipIf(date("w") == 0 || date("w") == 6);

    expect(true)->toBe(true);
});
```

### Injecting variables at root scope

To inject some variables to all scopes (e.g. database connection, helpers, etc.) and make it available in all you specs, one solution is to configure you `kahlan-config.php` file like the following:

```php
Filters::apply($this, 'run', function($next) {
    $scope = $this->suite()->root()->scope(); // The top most describe scope.
    $scope->global = 'MyVariable';
    return $next();
});
```

Then you can get it in any scopes like in the following:

```php
describe("My Spec", function() {
    it("echoes the global", function() {
        echo $this->global;
    });
});
```
