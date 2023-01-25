# Pretty-Print XML via PHP and JavaScript
When writing **PHP** and **JavaScript**, there are many contexts in which it may be desirable to pretty-print data output.

JSON pretty-print methods are well-known.

Here are two methods for pretty-printing XML output:

## Pretty-Print XML in PHP

```php

```

**Source:**

_____


## Pretty-Print XML in JavaScript

```js
const formatXML = (xmlString, tabLength = 2, newlineCount = 1) => {

  let tab = ' '.repeat(tabLength);
  let newline = '\r\n'.repeat(newlineCount);
  let indent = '';
  let formatted = '';
    
  xmlString.split(/>\s*</).forEach((node) => {
      
    indent = (node.match(/^\/\w/)) ? indent.substring(tab.length) : indent;
    formatted += indent + '<' + node + '>' + newline;
    indent += (node.match(/^<?\w[^>]*[^\/]$/)) ? tab : '';
  });
    
  formatted = (newlineCount === 0) ? formatted.replace(/\>\s+\</g, '><') : formatted;    
  return formatted.replaceAll('<<', '<').replaceAll('>>', '>');
}

let xmlString = '<?xml version="1.0"?><env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"><env:Header/><env:Body><ns2:checkVatResponse xmlns:ns2="urn:ec.europa.eu:taxud:vies:services:checkVat:types"><ns2:countryCode>NL</ns2:countryCode><ns2:vatNumber>805734958B01</ns2:vatNumber><ns2:requestDate>2023-01-25+01:00</ns2:requestDate><ns2:valid>true</ns2:valid><ns2:name>BOOKING.COM B.V.</ns2:name><ns2:address>HERENGRACHT 00597 1017CE AMSTERDAM</ns2:address></ns2:checkVatResponse></env:Body></env:Envelope>';

console.log(formatXML(xmlString));

```

**Source:**


