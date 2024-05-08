title: Import From Google Sheets
keywords: Jspreadsheet, Jexcel, javascript, grid, data grid, edition bar
description: Import from Google Sheets to create amazing JSS data grids.

![Import from Google Sheets](img/data-grid/google-sheets-importer.svg){.icon}

# Google Sheets Importer

The Google Sheets Importer extension for Jspreadsheet is a JavaScript plugin designed to simplify the import of data, styles, validations, and formatting from Google Sheets into a JSON object. This object can then populate a Jspreadsheet data grid, effectively bridging data between Google Sheets and local spreadsheet applications.

> **Google Sheets to JSON**  You can convert any public or private Google Sheets directly from its link to a local JSON with the appropriate permissions. It is required a Google API key.

## Documentation

Import public or private Google Sheets using the Sheet's URL or Sheet ID. It requires a proper Google Sheets API configuration, which can be done via the Google Console.  

### Methods

| Method                  | Description                                                                                                           |
| ------------------------|-----------------------------------------------------------------------------------------------------------------------|
| sheets(object?)         | Set the configuration for your google sheets extension.<br/>`sheets(config?: Object) => void`                         |
| sheets(string, object?) | Import a new Google Sheets spreadsheet into the JSS json format.<br/>`sheets(ident: String, config?: Object) => void` |

 

### Configuration

| Method           | Description                                                                                                                                                                                  |
| -----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| onload: Function | Method to be call after the import is finalized. You can use that to send the config to create a new Jspreadsheet Data Grid instance.<br/> `onload: (title: string, config: object) => void` |

 

#### Importing private sheets

| Method               | Description                                                |
| ---------------------|------------------------------------------------------------|
| clientId: string     | yourclientapihashidentification.apps.googleusercontent.com |
| apiKey: string       | Google API Key                                             |
| clientSecret: string | Google API Secret Client Key                               |
| redirectUri: string  | Google Sheets success URL return.                          |
| errorUri: string     | Google Sheets import error URL return.                     |

 

### Security notice

The Google Sheets API credentials can be restricted by domain, meaning they can only be used from a specific domain or set of domains. This provides some level of security. However, it is still advisable to take precautions to protect these credentials, even if they are domain-restricted. Please note that the following are just some possible additional measures to protect your keys, and it's your responsibility to make sure you implement a safe application: 

  * **Minimize exposure** : While domain restrictions help limit the usage of credentials to specific domains, it's still good practice to minimize the exposure of sensitive information as much as possible. Avoid storing them directly in your JavaScript source code or any publicly accessible files. 
  * **Secure server-side storage** : Store the credentials securely on your server-side code or in environment variables. Ensure that access to these credentials is restricted and follow best practices for secure storage. 
  * **Use HTTPS** : Make sure your website or application is served over HTTPS. This ensures that communication between the client and server is encrypted, reducing the risk of interception and unauthorized access to the credentials. 
  * **Password protection** : Implement limited access for authorized users. 

 
> **Important**  It's important to understand that the information above is not exhaustive, and as a developer, you hold the sole responsibility for ensuring the security of your application. We highly recommend further research on best security practices. 

 

### Sample configuration

{.ignore}
```javascript
sheets({
    // Required for public or private sheets
    clientId: 'xyz.apps.googleusercontent.com',
    // Only necessary when dealing with private Google Sheets
    apiKey: 'xyzxyzxyz-q1',
    clientSecret: 'xyz-xyzxyz_xyz',
    redirectUri: 'http://localhost:8000/sheets',
    errorUri: 'http://localhost:8000/sheets/error',
});

// Import from a Google Sheets
sheets('https://docs.google.com/spreadsheets/d/16Es5bj0dSfPXDUsbDfdUc7Y5mHkIL8Skux5YJLOenV0', {
    onload: function(title, config) {
        jspreadsheet(document.getElementById('spreadsheet'), config);
    }
});
```
 

## Examples

### Import from Google Sheets

Create a new JSS data grid from a remote public Google Sheets document. This example only works for public spreadsheets. 

```html
<html>
<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/sheets/dist/index.min.js"></script>

<div id='spreadsheet'></div>

<p><input type="text" value="https://docs.google.com/spreadsheets/d/16Es5bj0dSfPXDUsbDfdUc7Y5mHkIL8Skux5YJLOenV0" style="min-width: 320px" class="w100" /></p>

<input type='button' value='Import from Google Sheets' id="btn1">

<script>
// Defined your Google API configuration (You need more arguments to download private spreadsheets)
sheets({ apiKey: 'AIzaSyBFwiAJ2Ae50q2xPLdtaHlW4VM4UkFv-Q4' });
    // Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');
// Add-on for Spreadsheet
jspreadsheet.setExtensions({ sheets });
// Root
let root = document.getElementById('spreadsheet');
// Event
document.getElementById("btn1").addEventListener('click', function(e) {
    let url = e.target.previousElementSibling.firstChild.value;
    jspreadsheet.sheets(url, {
        onload: function(title, config) {
            // Destroy any existing data grid
            jspreadsheet.destroy(root);
            // Create a new data grid from the import process
            jspreadsheet(root, config);
        }
    })
});
</script>
</html>
```
```jsx
import React, { useRef } from "react";
import jspreadsheet from "jspreadsheet";
import sheets from "@jspreadsheet/sheets";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');
// Add-on for your JSS data grid
jspreadsheet.setExtensions({ sheets });
// Defined your Google API configuration (You need more arguments to download private spreadsheets)
sheets({ apiKey: 'yourGoogleApiHere' });

export default function App() {
    const input = useRef();
    const spreadsheet = useRef();

    const download = () => {
        jspreadsheet.sheets(document.getElementById('spreadsheet'), {
            onload: function(title, config) {
                jspreadsheet.sheets(spreadsheet.current, config);
            }
        });
    }

    return (
        <>
            <div ref={spreadsheet}></div>
            <input ref={input} type="text"
                value="https://docs.google.com/spreadsheets/d/16Es5bj0dSfPXDUsbDfdUc7Y5mHkIL8Skux5YJLOenV0" />
            <input type="button" value="Import from Google Sheets" onClick={() => convert(input.current.value)} />
        </>
    );
}
```
```vue
<template>
    <div ref="spreadsheet"></div>
    <input ref="input" type="text" value="https://docs.google.com/spreadsheets/d/16Es5bj0dSfPXDUsbDfdUc7Y5mHkIL8Skux5YJLOenV0" />
    <input type="button" value="Import from Google Sheets" @click="convert" />
</template>

<script>
import { jspreadsheet } from "@jspreadsheet/vue";
import sheets from "@jspreadsheet/sheets";

// Define the data grid license
jspreadsheet.setLicense('###license###');
// Define the data grid extensions
jspreadsheet.setLicense({ sheets });
// Defined your Google API configuration (You need more arguments to download private spreadsheets)
sheets({ apiKey: 'yourGoogleApiHere' });

export default {
    methods: {
        convert() {
            // Spreadsheet instance
            jspreadsheet.sheets(this.$refs.input.current, {
                onload: function(title, config) {
                    jspreadsheet(this.$refs.spreadsheet.current, options);
                }
            });
        }
    },
    data() {
        const input = ref(null);
        const spreadsheet = ref(null);

        return {
            spreadsheet,
            input
        };
    }
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";
import * as render from "@jspreadsheet/sheets";

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');
// Extensions
jspreadsheet.setExtensions({ sheets });
// Defined your Google API configuration (You need more arguments to download private spreadsheets)
sheets({ apiKey: 'yourGoogleApiHere' });

@Component({
  selector: "app-root",
  template: `<div #spreadsheet></div>
    <input #input type="text" value="https://docs.google.com/spreadsheets/d/16Es5bj0dSfPXDUsbDfdUc7Y5mHkIL8Skux5YJLOenV0" />
    <input type="button" value="Import from Google Sheets" (click)="this.convert()" />`
})
export class AppComponent {
    @ViewChild("input") input: ElementRef;
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    export() {
        jspreadsheet.sheets(this.input.value, {
            onload: function(title, config) {
                jspreadsheet(this.spreadsheet, options);
            }
        });
    }
}
```
 
