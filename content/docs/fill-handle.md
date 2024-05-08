title: Jspreadsheet Fill Handle
keywords: Jspreadsheet, Jexcel, data grid, JavaScript, Excel-like formulas, spreadsheet data, worksheet data, manage spreadsheet data, change cell data, update worksheet data
description: Learn more about the methods, events and configurations related to the Fill Handle features on Jspreadsheet.

# The Spreadsheet Fill Handle

The Jspreadsheet fill handle is a tool for quickly copying formulas or data up or down a column or across a row. It is identified by the small black square at the bottom-right corner of a cell selection.

## Documentation

### Settings

You can enable or disable the fill handle as below.

| Settings             | Description                                       |
|----------------------|---------------------------------------------------|
| fillHandle?: boolean | Enable or disable the fill-handle. Default: true  |

### Events

You can identify changes from a fill handle when the third argument `origin: "fill-hanlde"` available on the event `onafterchanges`

| Event                                                                                           |
|-------------------------------------------------------------------------------------------------|
| `onafterchanges?: (worksheet: worksheetInstance, records: Array<any>, origin: string) => void;` |

 

## Examples

### Disable the fill handle

```html
<div id="spreadsheet"></div>

<script>
jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        minDimensions: [6,6],
        fillHandle: false,
    }],
    onafterchanges: function(worksheet, records, origin) {
        console.log(origin, records);    
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

    const onAfterChanges = function(worksheet, records, origin) {
        console.log(origin, records)
    }

    // Render data grid component
    return (
        <Spreadsheet ref={spreadsheet} license={license} onafterchanges={onAfterChanges}>
            <Worksheet minDimensions={[6, 6]} fillHandle={false} />
        </Spreadsheet>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license" :onafterchanges="onAfterChanges">
        <Worksheet :minDimensions="[6, 6]" :fillHandle="false" />
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
        onAfterChanges(worksheet, records, origin) {
            console.log(records, origin)
        }
    },
    data() {
        return { license }
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
                minDimensions: [6,6],
                fillHandle: false,
            }],
            onafterchanges: function(worksheet, records, origin) {
                console.log(origin, records);    
            }
        });
    }
}
```
