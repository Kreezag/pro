title: Saving the Data Grid to Your Backend
keywords: Jexcel, javascript, cases, data persistence, database synchronization
description: Learn how data persistence can help you create exceptional online spreadsheets.

[Back to Use Cases](/docs/v7/examplese "Back to the use cases section")

# Data Persistence

With the persistence directive, each change in the data will be sent to a remote URL.

```html
<html>
<script src="https://jspreadsheet.com/v7/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v7/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<div id="spreadsheet"></div>

<script>
let mySpreadsheet = jspreadsheet(document.getElementById('spreadsheet'), {
    url:'/jspreadsheet/books.json',
    columns: [
      {
           type: 'autonumber',
           width: '50px',
           title: 'Code',
           name: 'id',
           readOnly: true,
           primaryKey: true
       },
       {
           type: 'text',
           width: '80px',
           title:'Image',
           name:'thumbnailUrl'
       },
       {
           type: 'text',
           width: '200px',
           title:'Title',
           name:'title'
       },
       {
          type: 'text',
           width: '55px',
           title:'Pages',
           name:'pageCount'
       },
       {
           type: 'calendar',
           width: '90px',
           title:'Published',
           name: 'publishedDate'
       },
      {
           type: 'text',
           width: '200px',
           title:'Author',
           name:'authors'
       },
       {
           type: 'dropdown',
           width: '180px',
           title:'Categories',
           name:'categories',
           source:['Internet','Web Development', 'Java', 'Mobile', 'Open Source'],
           multiple:true
      },
    ],
    allowComments:true,
    persistance: '/jspreadsheet/save',
    updateTable: function(instance, cell, col, row, val, label, cellName) {
        if (col == 1) {
             if (! val) {
               cell.innerHTML = '<img src="https://images-na.ssl-images-amazon.com/images/I/21%2Bwfxx2lyL._SX319_BO1,204,203,200_.jpg" style="width:60px;">';
            } else {
                cell.innerHTML = '<img src="' + val + '" style="width:60px;">';
            }
        }

        cell.style.overflow = 'hidden';
    },
    onevent:function() {
        console.log(arguments);
    },
    license: '###license###',
});
</script>
</html>
```
 
