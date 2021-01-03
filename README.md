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

----

Green text on a Node.JS REPL console means "this is what you need to type (or paste) into a Node.JS REPL console in order to obtain (define) this variable again". It is NOT a print-out (like `console.log` output) of that variable
=====================================================================================================================================================================================================================================

On Node.JS REPL (that is, Node.JS on terminal), if a string appears green, it is not a real (not an accurate) representation of the actual string. It is a representation of what you need to type (or paste) in to the terminal in order to obtain that string. On the example below, comments represent the value returned by Node.JS REPL as a result of an expression:

```javascript
let a = '\a\b\c\d\e\f\g\h\i\j';
// undefined
a
// 'a\bcde\fghij'
console.log(a);
// Following is not the value returned by Node.JS, but rather the output of `console.log` command:
// cde
//    ghij
```

Let's explain what happens in the script above:
1. We have declared a string and assigned this string to a variable named `a`. While declaring the string, for demonstration purposes, we have escaped every character using a backslash.
1. After declaring `a`, we have entered it to the REPL console and hit <kbd>Return</kbd>. As a result of this, Node.JS printed `'a\bcde\fghij'` (in green color). As you can observe, this is what we need to enter the Node.JS REPL console if we wanted to define this string again. The reason is, `a`, `c`, `d`, `e`, `g`, `h`, `i` and `j` are not "escape sequences". That is, these characters do not have special meanings when they are preceded with a backslash. On the other hand, `\b` and `\f` (as well as `\n` and some more other characters) are "escape sequences". That is, these characters have special meaning when they are preceded with a backslash. Hence, Node.JS indicated that they need to be entered with a preceding backslash in order to create this sting again. On the other hand, since the other characters are not escape sequences, in a string, they mean the same thing with or without a preceding backslash. That is, when declaring a string, `\a` and `a` are exactly the same, since `a` is not an escape sequence. Hence when displaying the `a` variable, Node.JS does not insert a backslash before the non-escape sequence characters.
1. When we print (log) the string, Node.JS outputs:

   ```shell
   cde
      ghij
   ```
   What happens here is:

   1. Node.JS prints the first character, `a`.
   1. The next character is `\b`, which is "backslash". Hence, Node.JS moves back one character, removing `a`.
   1. Node.JS prints `c`, `d` and `e`, since these are not special escape characters.
   1. The next character is `\f`, which is "form feed". Form feed means go down one line directly and to the same column. Hence, Node.JS does that.
   1. The rest of the characters are not special escape characters. Hence, Node.JS simply prints them.

Another example which involves reading from a file:

test.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Test Document</title>
</head>
<body>
  <h1>This is a test document</h1>
</body>
</html>
```

Open a Node.JS REPL console by typing `node` to a terminal and enter the following code:

```javascript
let data;

require('fs').readFile('./test.html', 'utf8', (_, fileContents) => {
  data = fileContents
});
```

This will read the file and store it in the variable named `data`. Then, type `data` to the console and press <kbd>Return</kbd> to obtain a Node.JS representation of the variable. The output is the following (and in green color because that's how Node.JS indicates that this is a representation):

```javascript
'<!DOCTYPE html>\n' +
  '<html lang="en">\n' +
  '<head>\n' +
  '  <meta charset="UTF-8">\n' +
  '  <title>Test Document</title>\n' +
  '</head>\n' +
  '<body>\n' +
  '  <h1>This is a test document</h1>\n' +
  '</body>\n' +
  '</html>\n'
```

As you can observe, Node.JS again gave us _what we need to type (or paste) in to the Node.JS REPL console in order to obtain the string represented by the variable `data`_. That is, the "test.html" file does not actually contain a backslash followed by the letter 'n'. Backslash and the letter 'n' is the escape sequence for a newline and Node.JS shows us that if we want to obtain that string again, that's what we need to type (or paste) in to the Node.JS REPL console.

For another example, let's say that we have the following file which contains JavaScript source code:

elements.js:

```javascript
var divElement = "<div class=\"someClassName\">Sample Text as DIV Content</div>\n";
```

Let's say that we need to read this file and obtain its contents, and store its contents into a variable. Hence, let's open a Node.JS REPL console and enter the following:

```javascript
let fileContents;

require('fs').readFile('elements.js', 'utf8', (err, data) => fileContents = data);
```

After doing this, when we enter `fileContents` on Node.JS REPL console to get a representation of the `fileContents` variable, Node.JS will give us the following (in green color since this is a representation, as opposed to being a print-out like console.log output):

```javascript
'var divElement = "<div class=\\"someClassName\\">Sample Text as DIV Content</div>\\n";\n'
```

That is, as we have indicated before, Node.JS is telling us that the line above is _what we need to type or paste in to Node.JS REPL console in order to obtain that variable_.

Now, you might be thinking "why do we have double backslashes in this representation? Also, why do we a double backslash followed by an `n` character?" The reason is, we have read the file into a variable. That is, even though the file happens to contain JavaScript code, we DID NOT evaluate (execute) that code. All we did was to simply read it, treating it absolutely no different than a regular string (like a `.txt` file). Hence, all contents of the file (which happens to be a JavaScript file) are read "as is". That is, all the quotes, backslashes, every single character are read "as is", without being evaluated to "escape sequences", etc. Hence, when we type `fileContents` on terminal, Node.JS gives us (in green text) what we need to type (or paste) in to the Node.JS REPL console to obtain that file contents. Typing double backslashes gives us a single backslash, since the backslash is a spacial character and hence needs to be escaped. When we escape a backslash (again, using a backslash because backslash is the escape character), we obtain a regular backslash.

Similarly, typing `\\n` will give us `\n`, instead of a real newline. Because that's exactly what we are supposed to get since the file contents are string, they are NOT JavaScript code for the `readFile` function.

Now, since the file contents are in fact, JavaScript code, let's try evaluating (running, executing) the file contents:

```javascript
eval(fileContents);
```

Now, let's type `divElement` to the console, which is a variable defined in the source file and get its representation (in green text) by Node.JS (below, comments represent the value returned by Node.JS REPL as a result of an expression):

```javascript
divElement
// '<div class="someClassName">Sample Text as DIV Content</div>\n'
```

As you can observe, Node.JS gives us what we need to type (or paste) in to the Node.JS REPL console in order to define this variable again. Note that Node.JS did not need to escape the double quotes in the string, because it used single quotes as the string container (just like it represented `'\a\b\c\d\e\f\g\h\i\j'` as `'a\bcde\fghij'`, instead of as it was defined). However, it still shows an `\n` as our file indeed ends with a newline.

Following is a recap of the last example. Remember that (most) comments represent Node.JS REPL outputs, either representations which are in green text, or print-outs, which are in white text:

elements.js:

```javascript
var divElement = "<div class=\"someClassName\">Sample Text as DIV Content</div>\n";
```

Node.JS REPL terminal:

```javascript
let fileContents;

require('fs').readFile('elements.js', 'utf8', (err, data) => fileContents = data);

fileContents
// (in green text)
// 'var divElement = "<div class=\\"someClassName\\">Sample Text as DIV Content</div>\\n";\n'

console.log(fileContents);
// (in white text)
// var divElement = "<div class=\"someClassName\">Sample Text as DIV Content</div>\n";
// <logs an empty line too>

// Evaluates what is shown by `console.log(fileContents)`. NOT what is
// shown by `fileContents`. The reason is that what is shown by
// `fileContents` is just Node.JS representation, as explained many
// times above, whereas what is shown by `console.log(fileContents)` is
// the real, true value of the string (which contains the whole contents
// (text) of the file).
eval(fileContents)

divElement
// (in green text)
// '<div class="someClassName">Sample Text as DIV Content</div>\n'

console.log(divElement)
// (in white text)
// <div class="someClassName">Sample Text as DIV Content</div>
// <logs an empty line too>
```

One more example:

```javascript
let a = 'abcd';
let b = '"abcd"';
a
// (in green text)
// 'abcd'
b
// (in green text)
// '"abcd"'
eval(a);
// (in white text)
// Uncaught ReferenceError: abcd is not defined
//     ...
eval(b);
// (in green text)
// 'abcd'
let c = eval(b);
console.log(c);
// (in white text)
// abcd
```

# Don't use `path.join()`. It is broken.

Use `[a, b, c].join(path.sep)` instead. The reason is, `path.join` ignores empty string elements. This means when you split a path and join it back, the root directory will be missing on Unix systems. Example:

```javascript
const folders = '/an/example/absolute/path'.split(path.sep);
// `folders` is `[ '', 'an', 'example', 'absolute', 'path' ]`
path.join(...folders);
// Returns `an/example/absolute/path` instead of `/an/example/absolute/path`
folders.join(path.sep);
// Returns `/an/example/absolute/path`
```

# `__dirname` does not return the link location on symbolic links
However, on hard links, they return the link location. Example:

/some/javascript/file/in/a/directory/link-test.js:

```javascript
console.log(__dirname);
```

A shell session in a directory named `/a/directory/that/is/not/the/directory/of/the/test/file`:

```shell
$ pwd
/a/directory/that/is/not/the/directory/of/the/test/file
$ node /some/javascript/file/in/a/directory/link-test.js
/some/javascript/file/in/a/directory
$ ln -s /some/javascript/file/in/a/directory/link-test.js
$ ls -l link-test.js
link-test.js -> /some/javascript/file/in/a/directory/link-test.js
$ node link-test.js
/some/javascript/file/in/a/directory
# Hoped it to print `/a/directory/that/is/not/the/directory/of/the/test/file` instead of `/some/javascript/file/in/a/directory`
$ rm link-test.js
$ ln /some/javascript/file/in/a/directory/link-test.js
$ ls -l link-test.js
link-test.js
$ node link-test.js
/a/directory/that/is/not/the/directory/of/the/test/file
```

As a side note, I think this is why `pnpm` uses hard links, instead of soft links.

# When you run an npm script, the working directory is always the root of the repository.

Example:

```shell
$ pwd
/an/absolute/path/to/some/project
$ ls -al
total 104
drwxr-xr-x  21 utku  staff   672  2 Jan 20:46 .
drwxr-xr-x  19 utku  staff   608  2 Jan 13:56 ..
drwxr-xr-x   4 utku  staff   128  2 Jan 20:21 node_modules
-rw-r--r--   1 utku  staff    45  2 Jan 20:45 package.json
drwxr-xr-x   8 utku  staff   256  2 Jan 20:48 some-directory
$ cat package.json 
{
  "scripts": {
    "dirTest": "asdf"
  }
}
$ tree -a node_modules/
node_modules/
├── .bin
│   └── asdf -> ../working-directory-test/test.js
└── working-directory-test
    └── test.js

2 directories, 2 files
$ cat node_modules/.bin/asdf 
#!/usr/bin/env node
console.log('Working directory is:');
console.log(process.cwd());
console.log();
console.log('__dirname is:');
console.log(__dirname);
$ cd some-directory/some-subdirectory
$ pwd
/an/absolute/path/to/some/project/some-directory/some-subdirectory
$ npx asdf
Working directory is:
/an/absolute/path/to/some/project/some-directory/some-subdirectory

__dirname is:
/an/absolute/path/to/some/project/node_modules/working-directory-test
$ npm run dirTest

> dirTest
> asdf

Working directory is:
/an/absolute/path/to/some/project

__dirname is:
/an/absolute/path/to/some/project/node_modules/working-directory-test
$ npx dirTest
npm ERR! code E404
npm ERR! 404 Not Found - GET https://registry.npmjs.org/dirTest - Not found
npm ERR! 404 
npm ERR! 404  'dirTest@latest' is not in the npm registry.
npm ERR! 404 This package name is not valid, because 
npm ERR! 404  1. name can no longer contain capital letters
npm ERR! 404 
npm ERR! 404 Note that you can also install from a
npm ERR! 404 tarball, folder, http url, or git url.

npm ERR! A complete log of this run can be found in:
npm ERR!     ~/.npm/_logs/2021-01-03T01_57_10_590Z-debug.log
$ npm run asdf
npm ERR! missing script: asdf

npm ERR! A complete log of this run can be found in:
npm ERR!     ~/.npm/_logs/2021-01-03T01_57_19_669Z-debug.log
$ 
```
