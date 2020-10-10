How to find where an NPM executable is coming from?
===================================================
1. Run `npm repo executable-name` to go to the source code repository of the executable.
1. In the repository, search for `"bin":` in every file named `package.json`. More specifically, search for the regex `"bin"[\s\n]*:`. This finds the `bin` property of in `package.json` files, which specify executables.
1. The value of the `bin` property gives you the executables.

Example:

1. `npm repo create-react-app`. GitHub page of the repository will open.
1. On the GitHub page of the repository, search for `bin`
1. When the search results come up, click on "Advanced search".
1. Under "Code options" -> "With this file name", enter "package.json".
1. In the updated search results, find the places in the `package.json` files with property `bin` (that is, places with `"bin":`.
