# docutils-parser

[![Build Status](https://travis-ci.com/vberlier/docutils-parser.svg?branch=master)](https://travis-ci.com/vberlier/docutils-parser)
![npm](https://img.shields.io/npm/v/docutils-parser.svg)

> Simple javascript parser for docutils xml documents.

This package uses [libxmljs](https://github.com/libxmljs/libxmljs) to parse and turn [docutils xml documents](http://docutils.sourceforge.net/docs/ref/doctree.html) into digestible javascript objects. This can be useful for working with documentation generated by tools like [sphinx](http://www.sphinx-doc.org).

```js
const docutils = require('docutils-parser');

const document = docutils.parseDocument(`
  <document source=".../hello.rst">
      <section ids="hello-world" names="hello,\\ world!">
          <title>Hello, world!</title>
          <paragraph>This file is empty.</paragraph>
      </section>
  </document>
`);

console.log(document.attributes);
// Output: { source: '.../hello.rst' }

const section = document.children[0];
console.log(section.children[0]);
// Output: { tag: 'title', attributes: {}, children: [ 'Hello, world!' ] }
```

## Installation

You can install `docutils-parser` with your `npm` client of choice.

```bash
$ npm install docutils-parser
```

## Usage

Elements in docutils documents are represented as plain javascript objects with a very simple structure:

- The `tag` property contains the name of the element
- The `attributes` property contains an object mapping attribute names to their associated values
- The `children` property is an array that can contain strings or other children elements

```js
{
  tag: 'parent',
  attributes: {
    'key': 'value'
  },
  children: [
    'some text',
    { tag: 'child', attributes: {}, children: [] }
  ]
}
```

The `parseDocument()` function takes a string and returns a hierarchy of elements that matches the content of the document. The element returned by the `parseDocument()` function is the root element of the document.

```js
const fs = require('fs');
const docutils = require('docutils-parser');

const content = fs.readFileSync('hello.xml', { encoding: 'utf-8' });
const document = docutils.parseDocument(content);

console.log(document.tag);
// Output: 'document'
```

## Contributing

Contributions are welcome. This project uses [jest](https://jestjs.io/) for testing.

```bash
$ npm test
```

The code follows the [javascript standard](https://standardjs.com/) style guide.

```bash
$ npm run lint
```

---

License - [MIT](https://github.com/vberlier/docutils-parser/blob/master/LICENSE)
