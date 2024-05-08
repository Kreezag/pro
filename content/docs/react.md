title: React Data Grid with Spreadsheet-Like Controls
keywords: Jspreadsheet, Jexcel, javascript, React, data grid, spreadsheet-like controls, React data grid, Jspreadsheet integration, React integration, JavaScript data grid, spreadsheet controls in React
description: Learn how to use Jspreadsheet and React to create robust data grids with spreadsheet-like controls. Explore the integration of Jspreadsheet in React applications for efficient data management and user-friendly interfaces.

# React Spreadsheet

The React wrapper for Jspreadsheet provides a bridge to incorporate advanced data grid functionalities within React applications. This wrapper simplifies embedding Jspreadsheet's interactive spreadsheet-like controls into React-based projects, focusing on seamless data manipulation and interface consistency. Designed for developers seeking to enhance application data grids with comprehensive features, the React wrapper aligns with modern development practices and user interaction standards. 

> **Updates**\
> From version 11, please import the CSS files as below.
> 
> `import "jsuites/dist/jsuites.css";`\
> `import "jspreadsheet/dist/jspreadsheet.css";`

## Documentation

### Utilizing the React Data Grid Wrapper

Begin by installing the JSS data grid React wrapper

```bash
$ npm install @jspreadsheet/react
```

### Crafting Your First JSS React Data Grid
Learn to construct a web-based spreadsheet employing the Jspreadsheet React wrapper.

{.ignore}
```jsx
import React, { useRef } from "react";
import { Spreadsheet, Worksheet, jspreadsheet } from "@jspreadsheet/react";
import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

// Define the global jspreadsheet license
jspreadsheet.setLicense('###license###');

export default function App() {
    // Spreadsheet array of worksheets
    const spreadsheet = useRef();

    // Render component
    return (
        <Spreadsheet ref={spreadsheet} tabs={true} toolbar={true}>
            <Worksheet />
        </Spreadsheet>
    );
}
```
 

### Direct Library Usage

Developers may utilize the library directly within their implementations for enhanced flexibility and customization.

#### React with Hooks

{.ignore}
```jsx
import React, { useRef, useEffect } from "react";
import jspreadsheet from "jspreadsheet";
import "jspreadsheet/dist/jspreadsheet.css";
import "jsuites/dist/jsuites.css";

// Define the global jspreadsheet license
jspreadsheet.setLicense('###license###');

export default function App() {
    const jssRef = useRef(null);

    useEffect(() => {
        // Create the spreadsheet only once
        if (!jssRef.current.jspreadsheet) {
            jspreadsheet(jssRef.current, {
                worksheets: [{
                    minDimensions: [10, 10]
                }],
            });
        }
    }, null);

    return (<div ref={jssRef} />);
}
```
 

#### React classes

{.ignore}
```jsx
import React from "react";
import jspreadsheet from "jspreadsheet";

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

jspreadsheet.setLicense('###license###');

export default class Jspreadsheet extends React.Component {
  constructor(props) {
    super(props);
    this.options = props.options;
    this.wrapper = React.createRef();
  }

  componentDidMount = function () {
    this.worksheets = jspreadsheet(this.wrapper.current, this.options);
  };

  addRow = function () {
    this.worksheets[0].insertRow();
  };

  render() {
    return (
      <div>
        <div ref={this.wrapper}></div>
        <p>
          <input type="button" value="Add new row" onClick={() => this.addRow()} className="jss_object" />
        </p>
      </div>
    );
  }
}

let options = {
  worksheets: [
    {
      minDimensions: [10, 10]
    }
  ],
};

ReactDOM.render(<Jspreadsheet options={options} />, document.getElementById("root"));
```
 

> ### Limitations
>
> #### setState utilization
> 
> In the current version of the wrapper, using setState to update the component automatically is not supported.   


## Library integration examples

### Codesandbox examples

#### React integration

* [React Spreadsheet](https://codesandbox.io/p/sandbox/react-components-on-jspreadsheet-zx9zxr)
Basic react spreadsheet with translations. 
* [React Spreadsheet Cell Editors](https://codesandbox.io/s/react-spreadsheet-with-a-custom-editor-ic6h3l)
How to create a custom cell editor with React. 
* [Custom React Components](https://codesandbox.io/s/react-components-on-jspreadsheet-k7wc4c)
Integrate a custom React component (Recharts) with Jspreadsheet. 
* [React Spreadsheet as React Classes](https://codesandbox.io/p/sandbox/react-spreadsheet-kkz3s8)
Create a basic react spreadsheet using React classes 
* [React Data Grid Validations](https://codesandbox.io/s/online-spreadsheet-with-validations-with-jspreadsheetxy777)
How to crate a data grid with cell validations
* [MUI React as a Custom Editor](https://codesandbox.io/p/sandbox/custom-editors-with-react-mui-y4v8lj)
React Calendar Antd
* [Antd React Calendar Cell Editor](https://stackblitz.com/edit/vitejs-vite-kwqcwy)
React Calendar with MUI
* [MUI React Calendar as a Custom Editor](https://codesandbox.io/p/sandbox/custom-editors-with-react-mui-forked-6hw4vz)


#### NextJS integration

* [Online XLSX NextJS reader](https://codesandbox.io/s/jspreadsheet-and-nextjs-6fhsz)
Creating an online XLSX reader with NextJS and Jspreadsheet 
* [Import a Excel file to NextJS](https://codesandbox.io/s/nextjs-spreadsheet-52mr2z)
How to import an excel file in NextJS using Jspreadsheet. 

