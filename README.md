nodemailer-mailgun-transport
============================

## What is this?
nodemailer is an amazing node module to send emails within any of your nodejs apps.
This is the transport plugin that goes with nodemailer to send email using [Mailgun](https://mailgun.com/)'s awesomeness!
Pow Pow.


## How does it work?
Based on [this mailgun-js module](https://github.com/1lobby/mailgun-js) and the [nodemailer module](https://github.com/andris9/Nodemailer), the Mailgun Transport was born. This is a transport layer, meaning it will allow you to send emails using nodemailer, using the Mailgun API instead of the SMTP protocol!

Nodemailer allows you to write code once and then swap out the transport so you can use different accounts on different providers. On top of that it's a super solid way of sending emails quickly on your node app(s).

The Mailgun transport for nodemailer is great to use when SMTP is blocked on your server or you just prefer the reliability of the web api!

## Quickstart - Example

Create a new file, install the dependencies **[1]** and look at the skeleton code below to get you started quickly!


```javascript
var nodemailer = require('nodemailer');
var mg = require('nodemailer-mailgun-transport');

// This is your API key that you retrieve from www.mailgun.com/cp (free up to 10K monthly emails)
var auth = {
  auth: {
    api_key: 'key-1234123412341234',
    domain: 'one of your domain names listed at your https://mailgun.com/app/domains'
  }
}

var nodemailerMailgun = nodemailer.createTransport(mg(auth));

nodemailerMailgun.sendMail({
  from: 'myemail@example.com',
  to: 'recipient@domain.com', // An array if you have multiple recipients.
  cc:'second@domain.com',
  bcc:'secretagent@company.gov',
  subject: 'Hey you, awesome!',
  'h:Reply-To': 'reply2this@company.com',
  //You can use "html:" to send HTML email content. It's magic!
  html: '<b>Wow Big powerful letters</b>',
  //You can use "text:" to send plain-text content. It's oldschool!
  text: 'Mailgun rocks, pow pow!'
}, function (err, info) {
  if (err) {
    console.log('Error: ' + err);
  }
  else {
    console.log('Response: ' + info);
  }
});
```

## Now with Consolidate.js templates

If you pass a "template" key an object that contains a "name" key, an "engine" key and, optionally, a "context" object, you can use Handlebars templates to generate the HTML for your message. Like so:

```javascript
var handlebars = require('handlebars');

var contextObject = {
  variable1: 'value1',
  variable2: 'value2'
};

nodemailerMailgun.sendMail({
  from: 'myemail@example.com',
  to: 'recipient@domain.com', // An array if you have multiple recipients.
  subject: 'Hey you, awesome!',
  template: {
    name: 'email.hbs',
    engine: 'handlebars',
    context: contextObject
  }
}, function (err, info) {
  if (err) {
    console.log('Error: ' + err);
  }
  else {
    console.log('Response: ' + info);
  }
});
```

You can use any of the templating engines supported by [Consolidate.js](https://github.com/tj/consolidate.js/). Just require the engine module in your script, and pass a string of the engine name to the `template` object. Please see the Consolidate.js documentation for supported engines.

**[1]** Quickly install dependencies
```bash
npm install nodemailer
npm install git+https://github.com/orliesaurus/nodemailer-mailgun-transport.git
```
