title: Create JSS Data Grids Directly from XLSX Files with JSS Parser
keywords: Jspreadsheet, Jexcel, javascript, import excel files, XLSX to JSS spreadsheet, JSS Parser, JavaScript plugin, data conversion, XLSX files, JSON format, spreadsheet import, spreadsheet to JSON, Excel to JSS data grid, data transformation, data management, spreadsheet solutions, data grid plugin
description: The JSS Parse extension enables smooth integration of XLSX files into JSS spreadsheets. Effortlessly create JSS spreadsheets by importing Excel data, facilitating seamless data management and organization.

![Import from XLSX](img/data-grid/import-from-xlsx.svg){.icon}

# Import from XLSX

The JSS parser is a JavaScript plugin that converts XLSX files to JSON format. It can be used as a standalone converter or to import XLSX files into the JSS data grid.  

## Documentation

### Settings

| Property            | Description                                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------|
| url?: string        | Load the XSLX from a remote URL. Bear in mind any potential CORS restrictions using this property. |
| locale?: string     | Define the decimal and thousand separator based on a locale.                                       |
| parseHTML?: boolean | Import embed cell style as HTML.                                                                   |
| file?: object       | load from a local file. This property is used along input type='file'                              |
| onload?: function   | When the file is loaded.                                                                           |
| onerror?: function  | Method to be called when something is wrong.                                                       |

 

## Installation

Please choose one of the following options 

### Using NPM



```bash
$ npm install @jspreadsheet/parser
```

### Using a CDN


{.ignore}
```html
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/parser/dist/index.min.js"></script>
```
  

## Example

```html
<html>
<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<script src="https://cdn.jsdelivr.net/npm/jszip@3.6.0/dist/jszip.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/parser/dist/index.min.js"></script>

<div id="spreadsheet"></div>

<input type="file" name="file" id='file' style='display:none'>

<p><input type='button' value='Load a XLSX file from my local computer' onclick="document.getElementById('file').click()"/></p>

<script>
// Set license
jspreadsheet.setLicense('###license###');
// Set extensions
jspreadsheet.setExtensions({ parser });
// Root
let root = document.getElementById('spreadsheet');
// Create the spreadsheet from a local file
let load = function(e) {
    // Parse XLSX file and create a new spreadsheet
    jspreadsheet.parser({
        file: e.target.files[0],
        // It would be used to updated the formats only
        locale: 'en-GB',
        onload: function(config) {
            jspreadsheet(root, config);
        },
        onerror: function(error) {
            alert(error);
        }
    });
}
document.getElementById("file").onchange = (e) => load(e)
</script>
</html>
```
```jsx
import React, { useRef } from "react";
import { jspreadsheet } from "@jspreadsheet/react";
import parser from "@jspreadsheet/parser";

// Set license
jspreadsheet.setLicense(
    "Yjc4YWJlYjdjNDY0OTU3NzY1ZjY0YmUxNDM4MThjNzU0NTAzYTc1MDlhOTA3NDhhZjI0NzIzYmZjZjJjNTgyNTNjZDM3MzIwY2Q2NDE2YmM0ZWQ4OTk5ZWI2YjI2NzU3OWQ4MGM3NjMyNzVhZTYzNTZhYTFiM2MyM2FkNjc2YTcsZXlKamJHbGxiblJKWkNJNklpSXNJbTVoYldVaU9pSktjM0J5WldGa2MyaGxaWFFpTENKa1lYUmxJam94TnpFeU9ESXhOakEwTENKa2IyMWhhVzRpT2xzaWFuTndjbVZoWkhOb1pXVjBMbU52YlNJc0ltTnZaR1Z6WVc1a1ltOTRMbWx2SWl3aWFuTm9aV3hzTG01bGRDSXNJbU56WWk1aGNIQWlMQ0ozWldJaUxDSnNiMk5oYkdodmMzUWlYU3dpY0d4aGJpSTZJak0wSWl3aWMyTnZjR1VpT2xzaWRqY2lMQ0oyT0NJc0luWTVJaXdpZGpFd0lpd2lkakV4SWl3aVkyaGhjblJ6SWl3aVptOXliWE1pTENKbWIzSnRkV3hoSWl3aWNHRnljMlZ5SWl3aWNtVnVaR1Z5SWl3aVkyOXRiV1Z1ZEhNaUxDSnBiWEJ2Y25SbGNpSXNJbUpoY2lJc0luWmhiR2xrWVhScGIyNXpJaXdpYzJWaGNtTm9JaXdpY0hKcGJuUWlMQ0p6YUdWbGRITWlMQ0pqYkdsbGJuUWlMQ0p6WlhKMlpYSWlMQ0p6YUdGd1pYTWlYU3dpWkdWdGJ5STZkSEoxWlgwPQ=="
);

// Set extensions
jspreadsheet.setExtensions({ parser });

export default function App() {
    // Spreadsheet array of worksheets
    const spreadsheet = useRef<HTMLDivElement>(null);
    const input = useRef<HTMLInputElement>(null);

    // Create the spreadsheet from a local file
    const load = function (e) {
        // Parse XLSX file and create a new spreadsheet
        jspreadsheet.parser({
            file: e.target.files[0],
            // It would be used to update the formats only
            locale: "en-GB",
            onload: function (config) {
                jspreadsheet(spreadsheet.current, config);
            },
            onerror: function (error) {
                alert(error);
            },
        });
    };

    // Render component
    return (
        <>
            <div ref={spreadsheet}></div>
            <input
                id="file"
                type="file"
                name="file"
                onChange={load}
                style={{ display: "none" }}
            />
            <input
                type="button"
                value="Load a XLSX file from my local computer"
                onClick={() => document.getElementById("file")?.click()}
            />
        </>
    );
}
```
```vue
<template>
  <div ref="spreadsheet"></div>
  <input
    type="file"
    name="file"
    ref="fileInput"
    @change="loadFile"
    style="display: none"
  />
  <input
    type="button"
    value="Load a XLSX file from my local computer"
    @click="triggerFileInput"
  />
</template>
  
<script>
import { jspreadsheet } from "@jspreadsheet/vue";
import parser from "@jspreadsheet/parser";

jspreadsheet.setLicense('###license###');

jspreadsheet.setExtensions({ parser });

export default {
  methods: {
    triggerFileInput() {
      this.$refs.fileInput.click();
    },
    loadFile(e) {
      const spreadsheetEl = this.$refs.spreadsheet;
      jspreadsheet.parser({
        file: e.target.files[0],
        locale: "en-GB",
        onload: function (config) {
          jspreadsheet(spreadsheetEl, config);
        },
        onerror: function (error) {
          alert(error);
        },
      });
    },
  },
};
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";
import * as parser from "@jspreadsheet/parser";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

// Set the extensions
jspreadsheet.setExtensions({ parser });

@Component({
    selector: "app-root",
    template: `<div #spreadsheet></div>
        <input #file type="file" name="file" style="display:none" />
        <input type="button" value="Load a XLSX file from my local computer" (click)="this.file.click()"/>`
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    @ViewChild("file") file: ElementRef;

    ngAfterViewInit() {
        let spreadsheet = this.spreadsheet.nativeElement;
        // Add event to the file input
        this.file.nativeElement.onchange = function (e) {
            // Parse XLSX file and create a new spreadsheet
            jspreadsheet.parser({
                file: e.target.files[0],
                locale: "en-GB",
                onload: function (config) {
                    jspreadsheet(spreadsheet, config);
                },
                onerror: function (error) {
                    alert(error);
                }
            });
        };
    }
}
```
 
