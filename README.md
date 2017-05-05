# unity3d-package-syncer

[![npm version](https://badge.fury.io/js/unity3d-package-syncer.svg)](https://badge.fury.io/js/unity3d-package-syncer)
[![Dependency Status](https://david-dm.org/rotorz/unity3d-package-syncer.svg)](https://david-dm.org/rotorz/unity3d-package-syncer)
[![devDependency Status](https://david-dm.org/rotorz/unity3d-package-syncer/dev-status.svg)](https://david-dm.org/rotorz/unity3d-package-syncer#info=devDependencies)

A command line utility that synchronizes asset files from npm packages into an appropriate
directory of a game project that is made using the Unity game engine.


## Installation

```sh
$ npm install --save unity3d-package-syncer
```

Add the following script to your `package.json` file:

```json
"sync": "unity3d--sync"
```

such that you have something similar to the following:

```json
{
  "name": "unity-project",
  "version": "1.0.1",
  "description": "",
  "scripts": {
    "sync": "unity3d--sync"
  },
  "dependencies": {
    "unity3d-package-syncer": "^1.0.1"
  }
}
```


## Usage

This command must be executed where the CWD (current working directory) is the root
directory of the Unity project. That is, the directory that contains "Assets/", "Library/"
and "package.json".


## Creating Unity packages for npm

The **unity3d-package-syncer** only considers npm packages that have the keyword
`unity3d-package` declared in their `package.json` file when checking and copying files
from the npm package into a Unity project.

The following is an example of how the `package.json` of such a package might look:

```json
{
  "name": "some-cool-package",
  "version": "1.0.0",
  "description": "A cool package for Unity projects which ...",
  "keywords": [
    "unity3d",
    "unity3d-package"
  ]
}
```

For an example checkout the [unity3d-package-example](https://github.com/rotorz/unity3d-package-example) repository.


## Which files are copied from the npm package?

All files in the package's `assets/` directory are copied into the root of the package
inside the Unity project.

Some *extra files* are also copied from the npm package into the Unity project:

  - `package.json` (required)

  - `LICENSE`

  - `README.md`

For example, the file structure of the following npm package:

```
/some-cool-package/
  |-- assets/
  |    |-- Logo.png
  |    |-- Logo.png.meta
  |-- docs/
  |    |-- getting-started.md
  |-- index.js
  |-- LICENSE
  |-- package.json
  |-- README.md
```

Results in the following directory structure in a Unity project:

```
/MyUnityProject/
  |-- Assets/
  |    |-- Plugins/
  |    |    |-- Packages/
  |    |    |    |-- some-cool-package/
  |    |    |    |    |-- Logo.png
  |    |    |    |    |-- Logo.png.meta       (Copied from npm package)
  |    |    |    |    |-- LICENSE
  |    |    |    |    |-- LICENSE.meta        (Unity generates this automatically)
  |    |    |    |    |-- package.json
  |    |    |    |    |-- package.json.meta   (Unity generates this automatically)
  |    |    |    |    |-- README.md
  |    |    |    |    |-- README.md.meta      (Unity generates this automatically)
  |    |    |    |-- some-cool-package.meta   (Unity generates this automatically)
  |-- Library/
  |-- package.json
```

The `*.meta` files that are automatically generated by Unity should be included for each
file that is included in the `assets/` directory of your npm package.

Since nothing in the Unity project should be referencing the *extra files*; there
shouldn't be any issues with these being automatically generated by Unity each time the
package is copied into your project.


## How does the **unity3d-package-syncer** know when an npm package needs to be copied?

- If the package has not yet been copied into the Unity project.

- If the version of the package already in the Unity project differs from the version of
  the currently installed npm package.


## How does the **unity3d-package-syncer** know when an npm package needs to be removed?

- If the package exists in the Unity project but is not declared in `package.json`.


## What happens if I modify the package files that were copied into the Unity project?

**IMPORTANT: Any modifications will be lost when the package is next updated or uninstalled.**

If you need to modify the files of the package then you will need to update the npm
package with the modifications and adjust the version number of the package.

If you want to modify the files of a package that you don't currently maintain, then you
will need to create a fork of the original package so that you can maintain that.

Packages with editor scripts that create asset files **SHOULD NOT** be storing these
inside the `{unity-project}/Assets/Plugins/Packages/` directory structure because these
files will be lost if the package is updated or uninstalled.

If your package needs to generate configuration and/or data files then they should be
stored in a different location in the Unity project. I would suggest that packages adopt
the following directory structure for generated data files for consistency with one
another:

```
{unity-project}/Assets/PackageData/{name-of-package}/{generated-data-files}
```


## Contribution Agreement

This project is licensed under the MIT license (see LICENSE). To be in the best
position to enforce these licenses the copyright status of this project needs to
be as simple as possible. To achieve this the following terms and conditions
must be met:

- All contributed content (including but not limited to source code, text,
  image, videos, bug reports, suggestions, ideas, etc.) must be the
  contributors own work.

- The contributor disclaims all copyright and accepts that their contributed
  content will be released to the public domain.

- The act of submitting a contribution indicates that the contributor agrees
  with this agreement. This includes (but is not limited to) pull requests, issues,
  tickets, e-mails, newsgroups, blogs, forums, etc.
