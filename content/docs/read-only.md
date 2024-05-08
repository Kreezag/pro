title: Data Grid Read only Cells
keywords: Jspreadsheet, javascript, plugins, spreadsheet, readonly cells, data protection
description: Learn how to make columns or individual cells read-only in Jspreadsheet, protecting data and preventing users from editing specific cells in your data grid.

# Data Grid Cells: Read Only State Management

## Documentation

### Methods

Below are methods designed to facilitate programmatic updates within your spreadsheets.

| Method                               | Description                                                                                                |
|--------------------------------------|------------------------------------------------------------------------------------------------------------|
| setReadOnly(object\|string, boolean) | Change the read only state of a cell.<br/>`setReadOnly(ident: HTMLElement\|string, state: boolean) : void` |
| isReadOnly(number,number)            | Check if a cell is read only.<br/>`isReadOnly(x: number, y: number) : boolean`                             |

## Examples

### Programmatic Updates

The example demonstrates setting a spreadsheet column or an individual data grid cell to read-only status.

```html
<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<div id="spreadsheet"></div>

<p><input type="button" value="Disabled B2" id="btn1"></p>

<script>
let toggle = function(b) {
    if (worksheets[0].isReadOnly('B2')) {
        worksheets[0].setReadOnly('B2', false);
        b.value = 'Disable B2';
    } else {
        worksheets[0].setReadOnly('B2', true);
        b.value = 'Enable B2';
    }
}

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

// Create a new spreadsheet
let worksheets = jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        data: [
            ['Mazda', 2001, 2000, 1],
            ['Peugeot', 2010, 5000, 1],
            ['Honda Fit', 2009, 3000, 1],
            ['Honda CRV', 2010, 6000, 0],
        ],
        columns: [
            { readonly: true }
        ],
        cells: {
            C1: { readonly: true }
        }
    }]
});

document.getElementById("btn1").onclick = (e) => toggle(e.target);
</script>
</html>
```
```jsx
import React, { useRef } from "react";
import { Spreadsheet, Worksheet } from "@jspreadsheet/react";
import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

// Set the license
const license = '###license###';

// Readonly state
const toggle = function(worksheet, button) {
    if (worksheet.isReadOnly('B2')) {
        worksheet.setReadOnly('B2', false);
        button.value = 'Disable B2';
    } else {
        worksheet.setReadOnly('B2', true);
        button.value = 'Enable B2';
    }
}

// Create the component
export default function App() {
    // Spreadsheet array of worksheets
    const spreadsheet = useRef();
    // Data
    const data = [
        ['Mazda', 2001, 2000, 1],
        ['Peugeot', 2010, 5000, 1],
        ['Honda Fit', 2009, 3000, 1],
        ['Honda CRV', 2010, 6000, 0],
    ]
    const columns = [
        { readonly: true }
    ]
    const cells = {
        C1: { readonly: true }
    }

    // Method
    const toggle = function(worksheet, button) {
        if (worksheet.isReadOnly('B2')) {
            worksheet.setReadOnly('B2', false);
            button.value = 'Disable B2';
        } else {
            worksheet.setReadOnly('B2', true);
            button.value = 'Enable B2';
        }
    }

    // Render data grid component
    return (
        <>
            <Spreadsheet ref={spreadsheet} license={license} toolbar>
                <Worksheet data={data} columns={columns} cells={cells} />
            </Spreadsheet>
            <input type={"button"} value={"Disabled B2"} onClick={() => toggle(spreadsheet.current[0], this)} />
        </>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license">
        <Worksheet :data="data" :columns="columns" :cells="cells" />
    </Spreadsheet>
    <input ref="button" type="button" value="Disabled B2" @click="toggle" />
</template>

<script>
import { Spreadsheet, Worksheet } from "@jspreadsheet/vue";
import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

// Set the license
const license = '###license###';

export default {
    components: {
        Spreadsheet,
        Worksheet,
    },
    methods: {
        toggle() {
            let worksheet = this.refs.spreadsheet.current[0];
            if (worksheet.isReadOnly('B2')) {
                worksheet.setReadOnly('B2', false);
                this.refs.button.value.value = 'Disable B2';
            } else {
                worksheet.setReadOnly('B2', true);
                this.refs.button.value.value = 'Enable B2';
            }
        }
    },
    data() {
        // Data
        const data = [
            ['Mazda', 2001, 2000, 1],
            ['Peugeot', 2010, 5000, 1],
            ['Honda Fit', 2009, 3000, 1],
            ['Honda CRV', 2010, 6000, 0],
        ]
        const columns = [
            { readonly: true }
        ]
        const cells = {
            C1: { readonly: true }
        }
        return {
            data,
            columns,
            cells,
            license,
        };
    }
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

@Component({
    selector: "app-root",
    template: `
        <div #spreadsheet></div>
        <input #button type='button' value='Disabled B2' (click)="this.toggle()" />`;
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    @ViewChild("button") button: ElementRef;
    // Worksheets
    worksheets: jspreadsheet.worksheetInstance[];
    // Data
    data: data;
    // Create a new data grid
    ngOnInit() {
        // Create spreadsheet
        this.worksheets = jspreadsheet(this.spreadsheet.nativeElement, {
            worksheets: [{
                data: [
                    ['Mazda', 2001, 2000, 1],
                    ['Peugeot', 2010, 5000, 1],
                    ['Honda Fit', 2009, 3000, 1],
                    ['Honda CRV', 2010, 6000, 0],
                ],
                columns: [
                    { readonly: true }
                ],
                cells: {
                    C1: { readonly: true }
                }
            }]
        });
    }
    toggle() {
        if (this.worksheets[0].isReadOnly('B2')) {
            this.worksheets[0].setReadOnly('B2', false);
            this.button.value = 'Disable B2';
        } else {
            this.worksheets[0].setReadOnly('B2', true);
            this.button.value = 'Enable B2';
        }
    }
}
```
