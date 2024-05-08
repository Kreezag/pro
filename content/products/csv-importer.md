title: Jspreadsheet General Data Grid CSV Importer 
keywords: Jspreadsheet, Jexcel, javascript, payment calculator spreadsheet, data grid, import csv, CSV to JSS, CSV import, column matching, data import, data mapping, spreadsheet solutions, data management, JavaScript plugin, data grid plugin
description: Compatible with React, Vue, and Angular, the JSS CSV Importer streamlines data import to data grids, offering efficient column mapping and integration with Jspreadsheet controls for versatile data management.

![Data Grid CSV Importer](img/data-grid/csv-importer.svg){.icon}

# CSV Importer

The CSV Importer extension is a JavaScript plugin tool designed to facilitate the importation of data from CSV files into JSS data grids, even when the column order in the CSV is not predetermined. This extension enables users to import CSV data efficiently, allowing for manual mapping each CSV column to its corresponding column in the data grid. 

## Documentation

### Methods

| Method            | Description                                                                                                                                                                                            |
| ------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| importer(object?) | To open the importer modal and specify the import destination, you must provide the instance of the worksheet to which the data should be imported.<br/>`importer(worksheetInstance?: Object) => void` |

 

### Installation

Please choose one of the following options

#### Using NPM

```bash
$ npm install @jspreadsheet/importer
```

#### Using a CDN

{.ignore}
```html
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/importer/dist/index.min.js"></script>
```
 

### Translation

It is possible to translate this extension using the following code. 

{.ignore}
```javascript
jspreadsheet.setDictionary({
    'CSV column': 'Coluna CSV',
    'Spreadsheet column': 'Coluna da Planilha',
    'Import loaded records': 'Importar os registros carregados',
    'Load a new CSV file': 'Carregar um novo arquivo CSV',
    'Advanced CSV': 'CSV Avançado',
    'Import CSV': 'Importar CSV',
    '(Do not import)': 'Não importar',
})
```
 

## Examples

### General JavaScript CSV importer

This extension adds a new button on the toolbar. 

Click in the icon _file_download_{.material-icons} or in the button above to import a new CSV file from your computer.

```html
<html>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@jsuites/css/dist/style.min.css" type="text/css" />
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lemonadejs/dist/lemonade.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/importer/dist/index.min.js"></script>

<div id='spreadsheet'></div>

<p><input type="button" value="Import to first worksheet" id="btn1" /></p>

<script>
// Set your JSS license key (The following key only works for one day)
jspreadsheet.license = '###license###';

// Add-on for Jspreadsheet
jspreadsheet.setExtensions({ importer });

// Create the spreadsheets
let grid = jspreadsheet(document.getElementById('spreadsheet'), {
    toolbar: true,
    worksheets: [{
        minDimensions: [10, 10],
        tableOverflow: true,
        tableHeight: 300,
        tableWidth: 720,
    }],
});

document.getElementById("btn1").onclick = () => importer(grid[0])
</script>
</html>
```
```jsx
import React, { useRef } from "react";
import { Spreadsheet, Worksheet } from "@jspreadsheet/react";
import importer from "@jspreadsheet/importer";

import "@jsuites/css/dist/style.css";

// Define the license
const license = '###license###';

// Define the extensions
const extensions = { importer };

export default function App() {
    // Spreadsheet array of worksheets
    const spreadsheet = useRef();
    // Render component
    return (
        <>
            <Spreadsheet ref={spreadsheet} license={license} extensions={extensions} toolbar="true">
                <Worksheet tableOverflow="true" tableHeight="300" tableWidth="720" />
            </Spreadsheet>
            <input type="button" value="Import CSV" onClick={() => importer(spreadsheet.current[0])} />
        </>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license" :extensions="extensions">
        <Worksheet :minDimensions="[10,10]" :tableOverflow="true" tableHeight="300" tableWidth="720" />
    </Spreadsheet>
    <input type="button" value="Import CSV" @click="import" />
</template>

<script>
import { Spreadsheet, Worksheet } from "@jspreadsheet/vue";
import importer from "@jspreadsheet/importer";

import "@jsuites/css/dist/style.css";

// Set the license
const license = '###license###';

// Define extensions
const extensions = { importer };

export default {
    components: {
        Spreadsheet,
        Worksheet,
    },
    methods: {
        import() {
            // Worksheet where the new data should be imported into.
            importer(this.$refs.spreadsheet.current[0]);
        }
    },
    data() {
        return {
            license,
            extensions,
        };
    }
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";
import * as importer from "@jspreadsheet/importer";

import "jsuites/dist/jsuites.css";
import "@jsuites/css/dist/style.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = '###license###';

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense(license);

// Extensions
jspreadsheet.setExtensions({ importer });

@Component({
  selector: "app-root",
  template: `<div #spreadsheet></div>
    <input type="button" value="Import CSV" (click)="this.import()" />`
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    // Worksheets
    worksheets: jspreadsheet.worksheetInstance[];
    // Create a new data grid
    ngAfterViewInit() {
        // Create spreadsheet
        this.worksheets = jspreadsheet(this.spreadsheet.nativeElement, {
          worksheets: [
            {
              minDimensions: [10, 10],
              tableOverflow: true,
              tableHeight: 300,
              tableWidth: 720,
            }
          ]
        });
    }
    import() {
        // Worksheet where the new data should be imported into.
        importer(this.worksheets[0]);
    }
}

```
 
