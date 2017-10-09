---
currentMenu: getting-started
---

## Getting Started

<a name="requirements"></a>
### Requirements
- PHP 5.5+
- [phpdbg](http://php.net/manual/en/debugger-about.php) or [Xdebug](http://xdebug.org/) (only required for code coverage analysis)


<a name="installation"></a>
### Installation
The recommended way to install Kahlan is with [Composer](http://getcomposer.org/) as a *development* dependency of your project.

```bash
composer require --dev kahlan/kahlan
```

Alternatively, you may manually add `"kahlan/kahlan": "~4.0"` to the `require-dev` dependencies within your `composer.json`.

The Kahlan package is installed with a vendor library and its binary accessible through `vendor/bin/kahlan` by default. However, you can [configure](https://getcomposer.org/doc/articles/vendor-binaries.md#can-vendor-binaries-be-installed-somewhere-other-than-vendor-bin-) its location like all other composer vendor binaries.

<a name="running-kahlan"></a>
## Running Kahlan
Once Kahlan is installed, you can run your tests (referred to as *specs*) with:

```bash
./vendor/bin/kahlan
```

For a full list of the options, see the [CLI Options](cli-options.md).


<a name="directory-structure"></a>
### Directory Structure
The recommended directory structure is to add a `spec` directory at the top level of your project. You may then place your *Spec* files within this directory. Spec files should have a `.spec.php` or `Spec.php` suffix. The `spec` directory should mirror the structure of your source code directory.

An example directory structure would be:

```
├── spec                       # The directory containing your specs
│   └── ClassA.spec.php
│   └── subdir
│       └── ClassB.spec.php
├── src                        # The directory containing your source code
│   └── ClassA.php
│   └── subdir
│       └── ClassB.php
├── composer.json
└── README.md
```

For basic, you can clone [https://github.com/sitrunlab/learn-kahlan](https://github.com/sitrunlab/learn-kahlan) to get started.
