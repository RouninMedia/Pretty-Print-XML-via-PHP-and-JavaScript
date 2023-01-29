# Pretty-Print XML (and HTML) via PHP and JavaScript
When writing **PHP** and **JavaScript**, there are many contexts in which it may be desirable to pretty-print data output.

**JSON** pretty-print methods are well-known.

Following are:

 - a method for pretty-printing **XML** output in **PHP**
 - a *bonus method* (based on the process above, but involving two hacks) for pretty-printing **HTML** output in **PHP**
 - two more methods for pretty-printing **XML** output in **JavaScript**:

## Pretty-Print XML in PHP

```php

function Format_XML($XML_String) {

  $DOMDocument = new \DOMDocument('1.0');
  $DOMDocument -> preserveWhiteSpace = true;
  $DOMDocument -> formatOutput = true;
  $DOMDocument -> loadXML($XML_String);
  
  return $DOMDocument -> saveXML($dom -> documentElement);
}
  
$My_Example_XML = '<my><example>XML</example></my>';
  
echo Format_XML($XML_String);

```

**Source:** *Adapted from* **[Pretty print XML in PHP ](https://www.phpdeveloper.org.uk/pretty-print-xml-in-php/)** *by Paul Waring.*

_____

## Pretty-Print HTML in PHP

By utilising two hacks, the script above may be modified to pretty-print **HTML**:

```php

```

_____


## Pretty-Print XML in JavaScript

### Version 1 (with more Regular Expressions)

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

**Source:** *See Stack Overflow Member **@arcturus**' response (https://stackoverflow.com/questions/376373/#answer-49458964) to **@Darin Dimitrov**'s question: ["Pretty printing XML with javascript"](https://stackoverflow.com/questions/376373/pretty-printing-xml-with-javascript). The script immediately above is a more concise refactor of **@arcturus**' answer.*

_______

### Version 2 (with fewer Regular Expressions)

```js
const formatXML2 = (xmlString, tabLength = 2, newlineCount = 1) => {
  
  let tab = ' '.repeat(tabLength);
  let newline = '\r\n'.repeat(newlineCount);
  let formatted = '';
  let indent = '';
  
  const nodes = xmlString.slice(1, -1).split(/>\s*</);
  formatted += (nodes[0][0] == '?') ? '<' + nodes.shift() + '>' + newline : '';
  
  for (let i = 0; i < nodes.length; i++) {
    const node = nodes[i];
    indent = (node[0] == '/') ? indent.slice(tab.length) : indent;
    formatted += indent + '<' + node + '>' + newline;
    indent += ((node[0] != '/') && (node[node.length - 1] != '/') && (node.indexOf('</') == -1)) ? tab : '';
  }

  formatted = (newlineCount === 0) ? formatted.replace(/\>\s+\</g, '><') : formatted;
  return formatted;
}

let xmlString = '<?xml version="1.0"?><env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"><env:Header/><env:Body><ns2:checkVatResponse xmlns:ns2="urn:ec.europa.eu:taxud:vies:services:checkVat:types"><ns2:countryCode>NL</ns2:countryCode><ns2:vatNumber>805734958B01</ns2:vatNumber><ns2:requestDate>2023-01-25+01:00</ns2:requestDate><ns2:valid>true</ns2:valid><ns2:name>BOOKING.COM B.V.</ns2:name><ns2:address>HERENGRACHT 00597 1017CE AMSTERDAM</ns2:address></ns2:checkVatResponse></env:Body></env:Envelope>';

console.log(formatXML(xmlString));

```

**Source:** *In a comment beneath the same answer by **@arcturus** (see above), Stack Overflow Member **@milahu** responded with a refactor of **@arcturus**' script: https://jsfiddle.net/fbn5j7ya/, eliminating several of the Regular Expressions. The script immediately above is a refactor of **@milahu**'s refactor.*


