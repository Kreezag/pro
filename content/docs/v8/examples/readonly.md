title: Readonly Data Grid Cells
keywords: Jspreadsheet, javascript, plugins, spreadsheet, editors, javascript richtext, html editor
description: Learn how to set columns or individual data grid cells to read-only mode.

[Back to Examples](/docs/v8/examples)

# Data Grid Readonly Cells

The example below defines a spreadsheet column or a single cell as read-only. 

```html
<script src="https://jspreadsheet.com/v8/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v8/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<div id="spreadsheet"></div>

<p><input type="button" value="Disabled B2" id="togglebtn"></p>

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

// Set the JSS spreadsheet license
jspreadsheet.setLicense('###license###');

// Create a new spreadsheet
let worksheets = jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        data: [
            ['Mazda', 2001, 2000, 1],
            ['Pegeout', 2010, 5000, 1],
            ['Honda Fit', 2009, 3000, 1],
            ['Honda CRV', 2010, 6000, 0],
        ],
        columns: [
            {
                type: 'text',
                title:'Description',
                width:'200px',
                readOnly:true,
            },
            {
                type: 'text',
                title:'Year',
                width:'200px'
            },
            {
                type: 'text',
                title:'Price',
                width:'100px',
                mask:'#.##',
            },
            {
                type: 'checkbox',
                title:'Automatic',
                width:'100px'
            },
        ],
    }],
    updateTable: function(worksheet, cell, x, y, source, value, label) {
        if (x == 2 && y == 2) {
            cell.classList.add('readonly');
        }
    }
});

document.getElementById("togglebtn").onclick = (e) => toggle(e.target);
</script>
</html>
```
 
