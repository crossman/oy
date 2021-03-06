# Oy [![npm version](https://badge.fury.io/js/oy-vey.svg)](http://badge.fury.io/js/oy-vey) [![Build Status](https://travis-ci.org/revivek/oy.svg?branch=master)](https://travis-ci.org/revivek/oy)

*Emails, oy vey!*

Render HTML emails on the server with React. Oy provides functionality to:

- Validate props against [email best-practices](https://github.com/revivek/oy/tree/master/src/rules) with `Oy` components.
- Render templates server-side with `Oy.renderTemplate`.

[Blog Post](http://oyster.engineering/post/124868558323/emails-oy-vey-render-emails-with-react)

**Feedback request**: The [next](https://github.com/revivek/oy/tree/next) branch contains the 1.0.0-alpha release. It aims to improve the library's usability. Open an issue with your feedback. Thanks!

## Installation

```
npm install --save oy-vey
```

## Example usage

### Replace table markup with validating Oy components

```js
import React from 'react';
import Oy from 'oy-vey';

const {Table, TBody, TR, TD} = Oy;

export default (props) => {
  return (
    <Table width={props.maxWidth}>
      <TBody>
        <TR>
          <TD align="center">
            {props.children}
          </TD>
        </TR>
      </TBody>
    </Table>
  );
};
```

### Compose higher level components like usual

```js
import React from 'react';

import MyLayout from './layout/MyLayout.jsx';
import BodyText from './modules/BodyText.jsx';

export default (props) => {
  return (
    <MyLayout>
      <BodyText>Welcome to Oy!</BodyText>
    </MyLayout>
  );
};
```


### Inject rendered code into HTML skeleton with Oy.renderTemplate

For example, if using Express.js:

```js
import express from 'express';
import React from 'react';
import Oy from 'oy-vey';

import GettingStartedEmail from './templates/GettingStartedEmail.jsx';

const server = express();
server.set('port', (process.env.PORT || 8887));

server.get('/email/oy', (req, res) => {
  const template = Oy.renderTemplate(<GettingStartedEmail />, {
    title: 'Getting Started with Foo',
    headCSS: '@media ...',
    previewText: 'Here is your guide...'
  });
  res.send(template);
});

server.listen(server.get('port'), () => {
  console.log('Node server is running on port', server.get('port'));
});
```

## Default components

The `Oy` namespace exposes the following components validated against email best practices: 

```
Table TBody TR TD Img A
```

If you want to circumvent this validation, you can use `Oy.Element` and pass the `tagName` prop to implement your own validated element. If you additionally don’t mind React stripping the attributes below, you can use React DOM `Element` objects (i.e. use `<table>` instead of `<Table>`).

## HTML attributes

In addition to the [attributes supported by React](https://facebook.github.io/react/docs/tags-and-attributes.html#html-attributes), these HTML attributes are supported on `Oy` components:

```
align background bgColor border vAlign
```

## Oy.renderTemplate API

`Oy.renderTemplate(<Template />, templateOptions)`

The `templateOptions` parameter is an object with the following fields:

```
title (string, required) - Used by clients if email is opened in a web page.
previewText (string, required) - Short description that appears in email clients
headCSS (string, optional) - CSS that belongs in `<head>`
lang (string, optional) - ISO language code
dir (string, optional) - Either 'ltr' or 'rtl'. 'ltr' is the default
```

**Note about <= v0.5.0**: `bodyContent` in `options` is no longer supported to avoid the insertion of arbitrary HTML into the email body.

## Contributing

```
# Test
npm test

# Compile from ES6 in src/ to ES5 in lib/
npm run compile
```

We welcome contributions. If there's some information missing or ideas for how to make Oy better, please
send a pull request, file an issue, or email [1.vivekpatel@gmail.com](mailto:1.vivekpatel@gmail.com).

The best place to start would be in contributing new rules. [A running wishlist of email validation rules are in the Issues section](https://github.com/oysterbooks/oy/issues?q=is%3Aopen+is%3Aissue+label%3A%22rule+wishlist%22).
