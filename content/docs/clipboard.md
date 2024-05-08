title: Jspreadsheet Clipboard
keywords: Jspreadsheet, Jexcel, data grid, JavaScript, Excel-like formulas, spreadsheet data, worksheet data, manage spreadsheet data, change cell data, update worksheet data
description: Learn more about the methods, events and configurations related to the clipboard, including copy and paste operations on the spreadsheets.

# The Spreadsheet Clipboard

The Jspreadsheet clipboard feature allows for copying formats, styles, and automatic formula updates between Data Grids. It supports data overwriting during paste actions and includes customization options and events, as discussed in this section. 

## Documentation

### Clipboard Management Methods

These methods are designed to assist with clipboard operations within the data grid.

| Method             | Description                                                                                                                                                                                                                                                                                                                                                |
| -------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| copy               | Copy the selected cells <br/>@param {boolean} Clear the data before pasting.<br/> `copy(cut: Boolean) => void`                                                                                                                                                                                                                                             |
| paste              | Paste the data to the current cursor position<br/> `paste(x: Number, y: Number, data: String\|String[[]]) => void`                                                                                                                                                                                                                                         |

 
### Clipboard Operation Events

These events are triggered during clipboard interactions involving spreadsheet data.

| Event                    | Description                                                                                                                                                                                                           |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| oncopy                   | `oncopy(worksheet: Object, highlighted: Boolean, str: String, cut: boolean)`<br/>The data copied to the clipboard.                                                                                                    |
| onbeforepaste            | `onbeforepaste(worksheet: Object, data: Array, x: Number, y: Number, properties: Object[])`<br/>It occurs before pasting data into the spreadsheet, allowing interception or modification.                            |
| onpaste                  | `onpaste(worksheet: Object, records: [])`<br/>After the paste action.                                                                                                                                                 |

> **Upgrades on Version 11**
> 
> In Version 11, the `onbeforepaste` and `onpaste` events were updated. These methods now handle arrays of objects when pasting occurs from another spreadsheet and arrays of data when pasting is from an external source.

## Examples

### Intercept Pasted Data

Intercept and cancel the paste event.

```html
<div id="spreadsheet"></div>

<script>
jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        data: [
            ['Mazda', 2001, 2000],
            ['Peugeot', 2010, 5000],
            ['Honda Fit', 2009, 3000],
            ['Honda CRV', 2010, 6000],
        ]
    }],
    onbeforepaste: function() {
        alert('Not allowed to paste');
        return false;
    }
});
</script>
```
```jsx
import React, { useRef } from "react";
import { Spreadsheet, Worksheet } from "@jspreadsheet/react";
import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = '###license###';

export default function App() {
    // Spreadsheet array of worksheets
    const spreadsheet = useRef();
    // Data
    const data = [
        ['Mazda', 2001, 2000],
        ['Peugeot', 2010, 5000],
        ['Honda Fit', 2009, 3000],
        ['Honda CRV', 2010, 6000],
    ];
    
    const onbeforepaste = function() {
        alert('Not allowed to paste');
        return false;
    }

    // Render data grid component
    return (
        <Spreadsheet ref={spreadsheet} license={license} onbeforepaste={onbeforepaste}>
            <Worksheet data={data} />
        </Spreadsheet>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license" :onbeforepaste="onbeforepaste">
        <Worksheet :data="data" />
    </Spreadsheet>
</template>

<script>
import { Spreadsheet, Worksheet } from "@jspreadsheet/vue";
import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = '###license###';

export default {
    components: {
        Spreadsheet,
        Worksheet,
    },
    data() {
        // Data
        const data = [
            ['Mazda', 2001, 2000],
            ['Peugeot', 2010, 5000],
            ['Honda Fit', 2009, 3000],
            ['Honda CRV', 2010, 6000],
        ];

        const onbeforepaste = function() {
            alert('Not allowed to paste');
            return false;
        }

        return {
            data,
            onbeforepaste,
            license,
        }
    }
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

// Create component
@Component({
    selector: "app-root",
    template: `<div #spreadsheet></div>`;
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    // Worksheets
    worksheets: jspreadsheet.worksheetInstance[];
    // Create a new data grid
    ngOnInit() {
        // Create spreadsheet
        this.worksheets = jspreadsheet(this.spreadsheet.nativeElement, {
            worksheets: [{
                data: [
                    ['Mazda', 2001, 2000],
                    ['Peugeot', 2010, 5000],
                    ['Honda Fit', 2009, 3000],
                    ['Honda CRV', 2010, 6000],
                ]
            }],
            onbeforepaste: function() {
                alert('Not allowed to paste');
                return false;
            }
        });
    }
}
```
