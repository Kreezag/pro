title: Spreadsheet Custom Contextmenu
keywords: Jspreadsheet, javascript, javascript vanilla, javascript plugin, plugin, excel-like, spreadsheet, table, tables, grid, datatables, data
description: How to use and customize the spreadsheet contextmenu. Learn how to add custom actions to your spreadsheet.

[Back to Examples](/docs/v5/examples "Back to the examples section")

# Custom contextmenu

The following example remove the copy and paste actions from the contextmenu.

```html
<html>
<script src="https://jspreadsheet.com/v5/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v5/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<div id="spreadsheet"></div>

<script>
let data1 = [
    ['US', 'Cheese', '2019-02-12'],
    ['CA', 'Apples', '2019-03-01'],
    ['CA', 'Carrots', '2018-11-10'],
    ['BR', 'Oranges', '2019-01-12'],
];

let mySpreadsheet = jspreadsheet(document.getElementById('spreadsheet'), {
    data: data1,
    columns: [
        { type: 'dropdown', url:'/jspreadsheet/countries', width:200, },
        { type: 'text', width:200, },
        { type: 'calendar', width:100, }
     ],
     allowComments: true,
     contextMenu: function(obj, x, y, e) {
         let items = [];

         if (y == null) {
             // Insert a new column
             if (obj.options.allowInsertColumn == true) {
                 items.push({
                     title:obj.options.text.insertANewColumnBefore,
                     onclick:function() {
                         obj.insertColumn(1, parseInt(x), 1);
                     }
                 });
             }

             if (obj.options.allowInsertColumn == true) {
                 items.push({
                     title:obj.options.text.insertANewColumnAfter,
                     onclick:function() {
                         obj.insertColumn(1, parseInt(x), 0);
                     }
                 });
             }

             // Delete a column
             if (obj.options.allowDeleteColumn == true) {
                 items.push({
                     title:obj.options.text.deleteSelectedColumns,
                     onclick:function() {
                         obj.deleteColumn(obj.getSelectedColumns().length ? undefined : parseInt(x));
                     }
                 });
             }

             // Rename column
             if (obj.options.allowRenameColumn == true) {
                 items.push({
                     title:obj.options.text.renameThisColumn,
                     onclick:function() {
                         obj.setHeader(x);
                     }
                 });
             }

             // Sorting
             if (obj.options.columnSorting == true) {
                 // Line
                 items.push({ type:'line' });

                 items.push({
                     title:obj.options.text.orderAscending,
                     onclick:function() {
                         obj.orderBy(x, 0);
                     }
                 });
                 items.push({
                     title:obj.options.text.orderDescending,
                     onclick:function() {
                         obj.orderBy(x, 1);
                     }
                 });
             }
         } else {
             // Insert new row
             if (obj.options.allowInsertRow == true) {
                 items.push({
                     title:obj.options.text.insertANewRowBefore,
                     onclick:function() {
                         obj.insertRow(1, parseInt(y), 1);
                     }
                 });
                 
                 items.push({
                     title:obj.options.text.insertANewRowAfter,
                     onclick:function() {
                         obj.insertRow(1, parseInt(y));
                     }
                 });
             }

             if (obj.options.allowDeleteRow == true) {
                 items.push({
                     title:obj.options.text.deleteSelectedRows,
                     onclick:function() {
                         obj.deleteRow(obj.getSelectedRows().length ? undefined : parseInt(y));
                     }
                 });
             }

             if (x) {
                 if (obj.options.allowComments == true) {
                     items.push({ type:'line' });

                     let title = obj.obj.getCellFromCoords(x, y).getAttribute('title') || '';

                     items.push({
                         title: title ? obj.options.text.editComments : obj.options.text.addComments,
                         onclick:function() {
                             obj.setComments([ x, y ], prompt(obj.options.text.comments, title));
                         }
                     });

                     if (title) {
                         items.push({
                             title:obj.options.text.clearComments,
                             onclick:function() {
                                 obj.setComments([ x, y ], '');
                             }
                         });
                     }
                 }
             }
         }

         // Line
         items.push({ type:'line' });

         // Save
         if (obj.options.allowExport) {
             items.push({
                 title: obj.options.text.saveAs,
                 shortcut: 'Ctrl + S',
                 onclick: function () {
                     obj.download();
                 }
             });
         }

         // About
         if (obj.options.about) {
             items.push({
                 title:obj.options.text.about,
                 onclick:function() {
                     alert(obj.options.about);
                 }
             });
         }

         return items;
     },
     license: '39130-64ebc-bd98e-26bc4',
});
</script>
</html>
```
 
