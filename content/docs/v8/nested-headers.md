title: Spreadsheet Nested Headers
keywords: Jspreadsheet, Jexcel, data grid, javascript, excel-like, spreadsheet, table, documentation, nested headers
description: How to create and manage nested headers and how to change those programmatically.

[Back to the Documentation](/docs/v8 "Back to the documentation section")

# Nested Headers

This section covers creating spreadsheets with `nested headers` and how to change them programmatically. 

## Documentation

### Methods

The following methods are available to update the nested headers programmatically.

| Method           | Description                                                                                                                                                                                                                                                                                           |
| -----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| getNestedCell    | Get a nested header cell (DOM element).<br/>`getNestedCell(x: Number, y: Number) : DOMElement`                                                                                                                                                                                                        |
| setNestedCell    | Define the nested header cell attributes<br/> `setNestedCell(x: Number, y: Number, properties: Object) : void` <br/>@param {number} x - coordinate x <br/>@param {number} y - coordinate y <br/>@param {object} properties - Possible attributes: { title: String, colspan: Number, tooltip: String } |
| getNestedHeaders | Return the nested header definitions.<br/>`getNestedHeaders() : Object`                                                                                                                                                                                                                               |
| setNestedHeaders | Set the nested header definitions.<br/>`setNestedHeaders(config: Object) : void`                                                                                                                                                                                                                      |

 

### Initial Settings

How to create a new spreadsheet with nested headers.

| Property                      | Description                                                                                                   |
| ------------------------------|---------------------------------------------------------------------------------------------------------------|
| nestedHeaders: array of items | Worksheet nested header definitions. Possible attributes: { title: String, colspan: Number, tooltip: String } |

 

### Available Events

| Event              | Description                                                                                                                             |
| -------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| onchangenested     | When changing the nested headers definitions<br/>`onchangenested(worksheet: Object, config: Object) : void`                             |
| onchangeheadercell | Update the properties of a nested header cell.<br/>`onchangeheadercell(worksheet: Object, x: Number, y: Number, config: Object) : void` |

 

## Examples

### Nested header example

The following example starts the spreadsheet with a basic nested headers configuration. 

   See a working example of a JSS [spreadsheet](https://jsfiddle.net/spreadsheet/0nwh5u71/) with nested headers that updates programmatically on JSFiddle.  

```html
<html>
<script src="https://jspreadsheet.com/v8/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />
<link rel="stylesheet" href="https://jspreadsheet.com/v8/jspreadsheet.css" type="text/css" />

<div id="spreadsheet"></div>

<br/><input type='button' value='Update' id="updatebtn" />

<script>
let data = [
    ['BR', 'Cheese', 1],
    ['CA', 'Apples', 0],
    ['US', 'Carrots', 1],
    ['GB', 'Oranges', 0],
];

let table = jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        data: data,
        columns: [
            {
                type: 'autocomplete',
                title: 'Country',
                width: '200px',
                url: '/jspreadsheet/countries'
            },
            {
                type: 'dropdown',
                title: 'Food',
                width: '100px',
                source: ['Apples','Bananas','Carrots','Oranges','Cheese']
            },
            {
                type: 'checkbox',
                title: 'Stock',
                width: '100px'
            },
            {
                type: 'number',
                title: 'Price',
                width: '100px'
            },
        ],
        minDimensions: [8,4],
        nestedHeaders:[
            [
                {
                    title: 'Supermarket information',
                    colspan: '8',
                },
            ],
            [
                {
                    title: 'Location',
                    colspan: '1',
                },
                {
                    title: ' Other Information',
                    colspan: '2'
                },
                {
                    title: ' Costs',
                    colspan: '5'
                }
            ],
        ],
        columnDrag:true,
    }]
});

document.getElementById('updatebtn').onclick = () => table[0].setNestedCell(0, 0, { title:'New title',tooltip:'New tooltip' });
</script>
</html>
```
 
