title: Data Grid Pagination
keywords: Jspreadsheet, Jexcel, data grid, JavaScript, data grid pagination, large data visualization, efficient data grid, high-performance data grid, paginated data grid, paginated data visualization
description: Efficiently handle large datasets in your HTML page with Jspreadsheet's worksheet pagination. Load and display millions of data records in a simple and efficient data grid, enhancing data visualization and optimizing user experience. Explore methods, settings, and configurations to customize pagination.

# Pagination

Jspreadsheet includes search and pagination to handle extensive spreadsheet data. It provides developers with customizable events and methods to seamlessly integrate pagination, optimizing application data navigation and display. 

## Documentation

### Methods

The following methods are related to pagination.

| Method         | Description                                                                                                                 |
| ---------------|-----------------------------------------------------------------------------------------------------------------------------|
| page           | `worksheet.page(pageNumber: Number) : void`<br/>Go to the page number.                                                      |
| whichPage      | `worksheet.whichPage(rowNumber?: Number) : void`<br/>Return the current page or which page you can find a row by its number |
| quantiyOfPages | `worksheet.quantiyOfPages() : number`<br/>Get the quantity of pages available.                                              |

 

### Events

The `onbeforesearch` can be used to intercept, change or cancel the search results.

| Event          | Description                                                                                                                                                                                                |
| ---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| onbeforesearch | `onbeforesearch(worksheet: Object, terms: String, results: Array, search: Object)`<br/>Action to be executed before the search. It can be used to cancel or to intercept and customize the search process. |
| onsearch       | `onbeforesearch(worksheet: Object, terms: String, rowNumber: Array, search: Object)`<br/>After the search process is completed.                                                                            |
| onchangepage   | `onbeforechangepage(worksheet: Object, newPage: Number, previousPage: Number, quantityPerPage: Number)`<br/>Action to be executed before the page changes. It can cancel the action by returning false.    |
| onpage         | `onchangepage(worksheet: Object, newPage: Number, previousPage: Number, quantityPerPage: Number)`<br/>After the page has changed.                                                                          |

 

### Initial Settings

Initial configuration related to search and the pagination of your data grid.

| Property                 | Description                                                        |
| -------------------------|--------------------------------------------------------------------|
| search: boolean          | Enable or disable the search.                                      |
| pagination: number       | The number of items per page                                       |
| paginationOptions: array | The options for the user to select the number of results per page. |

 

## Examples

### Data Grid Search and Pagination

How to enable the search and pagination features on spreadsheet initialization. 

```html
<html>
<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<div id="spreadsheet"></div>

<p><input type='button' value='Search for APP' id="btn1"/>
<input type='button' value='Go to the second page' id="btn2"/></p>

<script>
// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

// Create the spreadsheet
let spreadsheet = jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        csv: '/tests/demo.csv',
        csvHeaders: true,
        search: true,
        pagination: 10,
        paginationOptions: [10,25,50,100],
        columns: [
            { type:'text', width:80 },
            { type:'text', width:200 },
            { type:'text', width:100 },
            { type:'text', width:200 },
            { type:'text', width:100 },
        ],
    }],
    onchangepage: function(el, pageNumber, oldPage) {
        console.log('New page: ' + pageNumber);
    }
});

document.getElementById("btn1").onclick = () => spreadsheet[0].search('app');
document.getElementById("btn2").onclick = () => spreadsheet[0].page(1);
</script>
</html>
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
    // Columns
    const columns = [
        { type:'text', width:80 },
        { type:'text', width:200 },
        { type:'text', width:100 },
        { type:'text', width:200 },
        { type:'text', width:100 },
    ];
    // Event
    const onchangepage = (el, pageNumber, oldPage) => {
        console.log('New page: ' + pageNumber);
    }

    // Render component
    return (
        <Spreadsheet ref={spreadsheet} license={license} onchangepage={onchangepage}>
            <Worksheet columns={columns}
                csv="/tests/demo.csv"
                csvHeaders
                search
                pagination="10"
                paginationOptions={[10,25,50,100]}
            />
        </Spreadsheet>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license" :onchangepage="onchangepage">
        <Worksheet :columns="columns"
            csv="/tests/demo.csv"
            csvHeaders
            search
            pagination="10"
            :paginationOptions="[10,25,50,100]" />
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
    methods: {
        // Event
        onchangepage(el, pageNumber, oldPage) {
            console.log('New page: ' + pageNumber);
        }
    },
    data() {
        // Reference
        const spreadsheet = ref(null);

        // Columns
        const columns = [
            { type:'text', width:80 },
            { type:'text', width:200 },
            { type:'text', width:100 },
            { type:'text', width:200 },
            { type:'text', width:100 },
        ];

        return {
            spreadsheet,
            columns,
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
    template: `<div #spreadsheet></div>
    <input type='button' value='Search for APP' (click)="this.worksheets[0].search('app')" />
    <input type='button' value='Go to the second page' (click)="this.worksheets[0].page(1)" />`
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
                csv: '/tests/demo.csv',
                csvHeaders: true,
                search: true,
                pagination: 10,
                paginationOptions: [10,25,50,100],
                columns: [
                    { type:'text', width:80 },
                    { type:'text', width:200 },
                    { type:'text', width:100 },
                    { type:'text', width:200 },
                    { type:'text', width:100 },
                ],
            }],
            onchangepage: function(el, pageNumber, oldPage) {
                console.log('New page: ' + pageNumber);
            }
        });
    }
}
```

## Related Content

- [Data Grid Search](/docs/search)