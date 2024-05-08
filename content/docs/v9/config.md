title: Data Grid Configuration
keywords: Jspreadsheet, Jexcel, data grid, javascript, excel-like, spreadsheet, table, documentation, configuration
description: The configuration holds various properties to controls the behavior of the spreadsheet and its worksheets. Learn more about each of the available options.

[Back to the Documentation](/docs/v9 "Back to the documentation section")

# Data Grid Config

The configuration holds various properties to control the behavior of the spreadsheet and the worksheets. There are two objects, one config for the spreadsheet and one config for each of the worksheets. 

## Documentation

### Methods

The following methods perform operations for the config programmatically.

| Method    | Description                                                                      |
| ----------|----------------------------------------------------------------------------------|
| getConfig | Get the configuration of one worksheet.<br/>`getConfig() => Object`              |
| setConfig | Set the configuration for the worksheet<br/>`setConfig(options: Object) => void` |

 

### Spreadsheet config

One object is available for the spreadsheet level and holds all shared or global properties.

| Property                                                   | Description                                                                                                                                       |
| -----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| application?: string;                                      | Your application name                                                                                                                             |
| cloud?: string;                                            | Render a remote spreadsheet from Jspreadsheet Cloud, which is a serveless hosting service. That can be generate at https://jspreadsheet.com/cloud |
| root?: HTMLElement;                                        | DOM element for binding the javascript events. This property is normally used when JSS is running as a web component.                             |
| definedNames?: Record<string, string>,                     | Global defined names. It defines global range variables.                                                                                          |
| sorting?: (direction: boolean, column: number) => number;  | Global sorting handler.                                                                                                                           |
| server?: string;                                           | Remote URL for the persistence server                                                                                                             |
| toolbar?: boolean \| 'extended' \| Toolbar \| Function; | Enable the toolbars                                                                                                                               |
| editable?: boolean;                                        |  Allow table edition                                                                                                                              |
| allowExport?: boolean;                                     | Allow data export                                                                                                                                 |
| includeHeadersOnDownload?: boolean;                        | Include the table headers in the first row of the data                                                                                            |
| forceUpdateOnPaste?: boolean;                              | Force update on paste for read-only cells                                                                                                         |
| loadingSpin?: boolean;                                     | Enable loading spin when loading data. Default: false.                                                                                            |
| fullscreen?: boolean;                                      | Render jspreadsheet spreadsheet on full screen mode. Default: false                                                                               |
| secureFormulas?: boolean;                                  | Make sure the formulas are capital letter. Default: true                                                                                          |
| debugFormulas?: boolean,                                   | Enable formula debug. Default: false                                                                                                              |
| parseFormulas?: boolean;                                   | Execute formulas. Default: true                                                                                                                   |
| editorFormulas?: boolean;                                  | Disable the formula editor. Default: true                                                                                                         |
| autoIncrement?: boolean;                                   | Auto increment cell data when using the corner copy, including formulas, numbers and dates. Default: true                                         |
| autoCasting?: boolean;                                     | Try to cast numbers from cell values when executing formulas. Default: true                                                                       |
| stripHTML?: boolean;                                       | Remove any HTML from the data and headers. Default: true                                                                                          |
| tabs?: boolean \| Tabs;                                   | Allow tabs. Default: false                                                                                                                        |
| allowDeleteWorksheet?: boolean;                            | Allow the user to delete worksheets. Default: true                                                                                                |
| allowRenameWorksheet?: boolean;                            | Allow the user to rename worksheets. Default: true                                                                                                |
| allowMoveWorksheet?: boolean;                              | Allow the user to drag and drop the worksheets. Default: true                                                                                     |
| moveDownOnEnter?: boolean;                                 | Move the cursor down when pressing enter during edition. Default: true                                                                            |
| contextMenu?: Contextmenu;                                 | Return false to cancel the contextMenu event, or return custom elements for the contextmenu.                                                      |
| parseTableFirstRowAsHeader?: boolean;                      | The first row is the header titles when parsing a HTML table                                                                                      |
| parseTableAutoCellType?: boolean;                          | Try to identify a column type when parsing a HTML table                                                                                           |
| wordWrap?: boolean;                                        | Global cell wrapping. Default: false                                                                                                              |
| about?: string \| Function,                               | About information                                                                                                                                 |
| license?: string,                                          | License string                                                                                                                                    |
| worksheets?: Array<Worksheet>;                             | Worksheets                                                                                                                                        |
| validations?: any;                                         | Validations                                                                                                                                       |

 

### Worksheet options

There is one configuration for each worksheet.

| Property                                                                  | Description                                                                                                   |
| --------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| logo?: string                                                             | Logo URL                                                                                                      |
| url?: string;                                                             | Load the data from an external server URL                                                                     |
| persistence?: string \| boolean;                                         | Persistence URL or true when the URL is the same of the URL of the data source                                |
| sequence?: boolean;                                                       | Allow internal sequence for new rows                                                                          |
| data?: Array<Array<any>> \| Array<Record<string, any>>;                  | Load the data into a new spreadsheet from an array of rows or objects                                         |
| json?: Array<Record<string, any>>;                                        | Deprecated. Please use the data property.                                                                     |
| rows?: Row[];                                                             | Array with the rows properties definitions such as title, height.                                             |
| columns?: Array<Column>;                                                  | The column properties define the behavior of a column and the associated editor                               |
| cells?: Record<string, Column>;                                           | Define the properties of a cell. This property overwrite the column definitions                               |
| role?: string,                                                            | Role of this worksheet                                                                                        |
| nestedHeaders?: Array<Array<Nested>> \| Array<Nested>;                   | Nested headers definition                                                                                     |
| defaultColWidth?: number \| string;                                      | Default column width. Default: 50px                                                                           |
| defaultRowHeight?: number \| string;                                     | Default row height. Default: null                                                                             |
| defaultColAlign?: 'center' \| 'left' \| 'right' \| 'justify';          | Deprecated. The default alignment of a cell is defined by a CSS class from 8.2.0+                             |
| minSpareRows?: number;                                                    | Minimum number of spare rows. Default: 0                                                                      |
| minSpareCols?: number;                                                    | Minimum number of spare cols. Default: 0                                                                      |
| minDimensions?: [number, number];                                         | Minimum table dimensions: [numberOfColumns, numberOfRows]                                                     |
| csv?: string;                                                             | CSV data source URL                                                                                           |
| csvFileName?: string;                                                     | CSV default filename for the jspreadsheet exports. Default: 'jspreadsheet'                                    |
| csvHeaders?: boolean;                                                     | Consider first line as header. Default: true                                                                  |
| csvDelimiter?: string;                                                    | Delimiter to consider when dealing with the CSV data. Default: ','                                            |
| columnSorting?: boolean;                                                  | Allow column sorting                                                                                          |
| columnDrag?: boolean;                                                     | Allow column dragging                                                                                         |
| columnResize?: boolean;                                                   | Allow column resizing                                                                                         |
| rowResize?: boolean;                                                      | Allow row resizing                                                                                            |
| rowDrag?: boolean;                                                        | Allow row dragging                                                                                            |
| editable?: boolean;                                                       | Allow table edition                                                                                           |
| allowInsertRow?: boolean;                                                 | Allow new rows                                                                                                |
| allowManualInsertRow?: boolean;                                           | Allow new rows to be added using tab key. Default: true                                                       |
| allowInsertColumn?: boolean;                                              | Allow new columns to be added using enter key. Default: true                                                  |
| allowManualInsertColumn?: boolean;                                        | Allow new rows to be added via script. Default: true                                                          |
| allowDeleteRow?: boolean;                                                 | Allow rows to be deleted. Default: true                                                                       |
| allowDeletingAllRows?: boolean;                                           | Allow all rows to be deleted. Warning: no rows left can lead to undesirabled behavior. Default: false         |
| allowDeleteColumn?: boolean;                                              | Allow columns to be deleted. Default: true                                                                    |
| allowRenameColumn?: boolean;                                              | Allow rename column. Default: true                                                                            |
| allowComments?: boolean;                                                  | Allow users to add comments to the cells. Default: false                                                      |
| selectionCopy?: boolean;                                                  | Corner selection and corner data cloning. Default: true                                                       |
| mergeCells?: Record<string, any[]>;                                       | Merged cells. Default: null                                                                                   |
| search?: boolean;                                                         | Allow search on the spreadsheet                                                                               |
| pagination?: number;                                                      | Activate pagination and defines the number of records per page. Default: false                                |
| paginationOptions?: boolean \| Array<number>;                            | Dropdown for the user to change the number of records per page. Example: [10,25,50,100]. Default: false       |
| textOverflow?: boolean;                                                   | Text Overflow. Default: false                                                                                 |
| tableOverflow?: boolean;                                                  | Table overflow. Default: false                                                                                |
| tableHeight?: number \| string;                                          | Define the table overflow height. Example: '300px'                                                            |
| tableWidth?: number \| string;                                           | Define the table overflow width. Example: '800px'                                                             |
| comments?: Record<string, string>;                                        | Initial comments. Default: null                                                                               |
| meta?: Record<string, any>;                                               | Initial meta information. Default: null                                                                       |
| style?: Record<string, string>;                                           | Style                                                                                                         |
| freezeColumns?: number;                                                   | Freeze columns. Default: 0                                                                                    |
| orderBy?: [number, boolean];                                              | Initial sorting [colNumber, direction]. Default: null                                                         |
| worksheetId?: string;                                                     | Worksheet Unique Id.                                                                                          |
| worksheetName?: string;                                                   | Worksheet Name.                                                                                               |
| worksheetState?: 'hidden' \| undefined;                                  | Worksheet state: hidden \| null. Hide a worksheet                                                            |
| filters?: boolean;                                                        | Enable the column filters                                                                                     |
| footers?: Array<any>;                                                     | Footers                                                                                                       |
| applyMaskOnFooters?: boolean;                                             | Apply mask on footers                                                                                         |
| pluginOptions?: Record<string, any>;                                      | Define options for the plugins. Each key should be the pluginName.                                            |
| locked?: boolean;                                                         | This is a internal controller for the spreadsheet locked properties. Please use editable to make it readonly. |
| selectUnLockedCells?: boolean;                                            | Allow the selection of unlocked cells. Default: true.                                                         |
| selectLockedCells?: boolean;                                              | Allow the selection of locked cells. Default: true.                                                           |
| resize?: 'horizontal' \| 'vertical' \| 'both' \| 'none' \| undefined; | Enable resizable worksheet in on or both direction (horizontal \| vertical \| both). Default: none          |

 

## Examples

### Data grid manual persistence

The example below shows how to get the configuration of the data grid and create that later 

```html
<html>
<script src="https://jspreadsheet.com/v9/jspreadsheet.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v9/jspreadsheet.css" type="text/css" />
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<div id="spreadsheet"></div>
<div id="spreadsheet-clone"></div><br>

<textarea id="console" style="width: 600px; height: 100px;"></textarea><br>

<input type="button" value="Clone the data grid above" class="button main" id="clonebtn" />


<script>
// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

let clone = function() {
    // Get the data grid configuration
    let config = grid[0].parent.getConfig();
    // Show on the textarea
    document.getElementById('console').value = JSON.stringify(config);
    // Destroy any existing spreadsheet
    jspreadsheet.destroy(document.getElementById('spreadsheet-clone'));
    // Create a new spreadsheet
    jspreadsheet(document.getElementById('spreadsheet-clone'), config);
}

// Create the JavaScript sample data grid
let grid = jspreadsheet(document.getElementById('spreadsheet'), {
    tabs: true,
    toolbar: true,
    worksheets: [{
        data: [[1,2,3]],
        minDimensions: [8, 8],
    }],
});

document.getElementById('clonebtn').onclick = clone
</script>
</html>
```
 

See this [data grid](https://jsfiddle.net/spreadsheet/xz5pfgq7/) cloning working example on JSFiddle
