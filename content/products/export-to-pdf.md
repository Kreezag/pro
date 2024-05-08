title: Export to PDF
keywords: Jspreadsheet, Jexcel, javascript, grid, data grid, export to pdf, pdf generation
description: Jspreadsheet Print is an extension to generate a PDF from a jspreadsheet data grid.

![Export to PDF](img/data-grid/print.svg){.icon}

# Export to PDF

The JSS Export to PDF extension is designed for seamless integration with the JSS spreadsheet data grid plugin, enabling users to efficiently export their spreadsheets as PDFs in any language, provided the appropriate TTF font is loaded. This extension ensures easy data sharing and maintains the integrity of the spreadsheet's original structure during conversion, making it a versatile solution for multi-language applications. 

## Documentation

### Methods

| Method                | Description                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| print(object?)        | To open the print modal programmatically, you must specify the target spreadsheet instance or an array of worksheets. If no instance is provided, the function will use the currently active spreadsheet by default. However, an alert will be displayed to notify the user if no spreadsheet is in focus.<br/>`print(spreadsheet?: Object\|Object[]) => void` |
| print.setFont(string) | To customize the language you want to print, you need to define a TTF font that includes all the symbols and characters in your language.<br/>`print.setFont(ttf: String) => void`                                                                                                                                                                              |

 

### Installation

Please choose one of the following options 

#### Using NPM

```bash
npm install @jspreadsheet/print
```
 
#### Using a CDN

{.ignore}
```html
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/print/dist/index.min.js"></script>
```
 

## International

To export a spreadsheet in a language other than English, you need to define a TTF font that includes all the symbols and characters required for that language. You can then download and load your ttf font using `setFont(string)`. The following resources may be helpful. 

  * [Export in Japanese with Pretendard Regular](https://www.freejapanesefont.com/pretendard-jp-download/)
  * [Export in Korean using Noto Sans Korean](https://fonts.google.com/noto/specimen/Noto+Sans+KR)
  * [Export in Chinese with FengGuangMingRui](https://chinesefonts.org/fonts/fengguangmingrui-regular)
  * [Export in Hindu with Noto Sans Devanagari](https://fonts.google.com/noto/specimen/Noto+Sans+Devanagari)

 

#### Example

{.ignore}
```javascript
print.setFont('./Pretendard-Regular.ttf');
```  

## Examples

### Basic example of customizing fonts

Select a language below, the corresponding font will be set to display values of the spreadsheet in the PDF. 

```html
<html>
<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<script src="https://jsuites.net/v5/jsuites.js"></script>
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />

<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<script src="https://cdn.jsdelivr.net/npm/lemonadejs/dist/lemonade.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/print/dist/index.min.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/pdf-lib/dist/pdf-lib.js"></script>
<script type="text/javascript" src="https://unpkg.com/@pdf-lib/fontkit@1.1.1"></script>
<script type="text/javascript" src="https://unpkg.com/regenerator-runtime@0.13.1/runtime.js"></script>

<div id="spreadsheet"></div><br><br>

<select id='selectedLanguage' class='jss_object'>
    <option value='la'>Latin script</option>
    <option value='jp'>Japanese</option>
    <option value='ch'>Chinese</option>
    <option value='kr'>Korean</option>
    <option value='in'>Hindi</option>
</select>

<input type="button" id="btn1" class="jss_object" value="Print">

<script>
const dataExamples = {
  la: [["Hello", "World"]],
  jp: [["ハロー・", "ワールド"]],
  kr: [["안녕", "세상"]],
  ch: [["你好", "世界"]],
  in: [["नमस्ते", "दुनिया"]],
};

let changeLanguage = function(event) {
    let lang = event.target.value
    spreadsheet[0].setData(dataExamples[lang])
    
    if (lang === 'jp' || lang === 'kr' || lang === 'la') {
        // Pretendard Regular || Free use font from https://fontmeme.com/fonts/pretendard-jp-font/
        print.setFont('./font/Pretendard-Regular.ttf')
    } else if (lang === 'ch') {
        // FengGuangMingRui Regular || Download font from https://chinesefonts.org/fonts/fengguangmingrui-regular
        print.setFont('./font/FengGuangMingRui-Regular.ttf')
    } else if (lang === 'in') {
        // Noto Sans Regular || Free use font from https://fonts.google.com/noto/specimen/Noto+Sans
        print.setFont('./font/NotoSans-Regular.ttf')
    }
}

// Set the license for both plugin and the spreadsheet
jspreadsheet.setLicense('###license###');

// Set the extensions
jspreadsheet.setExtensions({ print });

// Create the spreadsheet
const spreadsheet = jspreadsheet(document.getElementById('spreadsheet'), {
    worksheets: [{
        data: [['Hello', 'World']],
        minDimensions: [6, 6],
    }]
});

spreadsheet[0].setData(dataExamples['la'])

document.getElementById("btn1").onclick = () => print(spreadsheet[0])
document.getElementById("selectedLanguage").onchange = (e) => changeLanguage(e)
</script>
</html>
```
```jsx
import React, { useRef, useState } from "react";
import { Spreadsheet, Worksheet } from "@jspreadsheet/react";
import jspreadsheet from "jspreadsheet";
import print from "@jspreadsheet/print";

// License
const license = '###license###';

// Extensions
const extensions = { print };

export default function App() {
    const [lang, setLang] = useState('la')
    const spreadsheet = useRef();

    const dataExamples = {
      la: [["Hello", "World"]],
      jp: [["ハロー・", "ワールド"]],
      kr: [["안녕", "세상"]],
      ch: [["你好", "世界"]],
      in: [["नमस्ते", "दुनिया"]],
    };

    const data = [...dataExamples.la]

    const changeLanguage = (event) => {
        const newLanguage = event.target.value
        spreadsheet.current[0].setData(dataExamples[newLanguage])
        setLang(newLanguage)

        if (newLanguage === 'jp' || newLanguage === 'kr' || newLanguage === 'la') {
            print.setFont('./fonts/Pretendard-Regular.ttf')
        } else if (newLanguage === 'ch') {
            print.setFont('./fonts/FengGuangMingRui-Regular.ttf')
        } else if (newLanguage === 'in') {
            print.setFont('./fonts/NotoSans-Regular.ttf')
        }
    }

    return (
        <>
            <Spreadsheet ref={spreadsheet} license={license} extensions={extensions} toolbar>
                <Worksheet data={data} />
            </Spreadsheet>
            <select id='selectedLanguage' onChange={(event) => changeLanguage(event)} value={lang}>
                <option value='la'>Latin script</option>
                <option value='jp'>Japanese</option>
                <option value='ch'>Chinese</option>
                <option value='kr'>Korean</option>
                <option value='in'>Hindi</option>
            </select>
            <button onClick={() => print(spreadsheet.current[0])}>Export/Print</button>
        </>
    );
}
```
```vue
<template>
    <Spreadsheet ref="spreadsheet" :license="license" :extensions="extensions">
        <Worksheet :data="data"></Worksheet>
    </Spreadsheet>
    <select id="selectedLanguage" @change="changeLanguage" v-model="lang">
        <option value="la">Latin script</option>
        <option value="jp">Japanese</option>
        <option value="ch">Chinese</option>
        <option value="kr">Korean</option>
        <option value="in">Hindi</option>
    </select>
    <button @click="print">Export/Print</button>
</template>

<script>
import { Spreadsheet, Worksheet, jspreadsheet } from "@jspreadsheet/vue";
import print from "@jspreadsheet/vue";

const dataExamples = {
  la: [["Hello", "World"]],
  jp: [["ハロー・", "ワールド"]],
  kr: [["안녕", "세상"]],
  ch: [["你好", "世界"]],
  in: [["नमस्ते", "दुनिया"]],
};

const license = '###license###';

export default {
    name: "App",
    components: {
        Spreadsheet,
        Worksheet,
    },
    data() {
        return {
            lang: "la",
            license: license,
            extensions: {
                print
            },
            data: [],
        };
    },
    created() {
        this.data = [...dataExamples["la"]];
    },
    methods: {
        print() {
            print(this.$refs.spreadsheet.current[0]);
        },
        changeLanguage(event) {
            const newLanguage = event.target.value;
            this.lang = newLanguage;
            this.$refs.spreadsheet.current[0].setData(dataExamples[this.lang]);

            if (newLanguage === "jp" || newLanguage === "kr" || newLanguage === "la") {
                print.setFont("./fonts/Pretendard-Regular.ttf");
            } else if (newLanguage === "ch") {
                print.setFont("./fonts/FengGuangMingRui-Regular.ttf");
            } else if (newLanguage === "in") {
                print.setFont("./fonts/NotoSans-Regular.ttf");
            }
        },
    },
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import * as jspreadsheet from "jspreadsheet";
import * as print from "@jspreadsheet/print";

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

// Set your JSS license key (The following key only works for one day)
jspreadsheet.setLicense('###license###');

// Extensions
jspreadsheet.setExtensions({ print });

const dataExamples = {
  la: [["Hello", "World"]],
  jp: [["ハロー・", "ワールド"]],
  kr: [["안녕", "세상"]],
  ch: [["你好", "世界"]],
  in: [["नमस्ते", "दुनिया"]],
};

@Component({
    selector: "app-root",
    template: `<div #spreadsheet></div>
    <select id="selectedLanguage" (change)="changeLanguage($event)" [(ngModel)]="lang">
        <option value="la">Latin script</option>
        <option value="jp">Japanese</option>
        <option value="ch">Chinese</option>
        <option value="kr">Korean</option>
        <option value="in">Hindi</option>
    </select>
    <button (click)="print(this.worksheet)>Export/Print</button>`
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    // Create a new data grid
    ngAfterViewInit() {
        // Create spreadsheet
        this.worksheets = jspreadsheet(this.spreadsheet.nativeElement);
    }

    changeLanguage(event: any) {
      const newLanguage = event.target.value;
      this.lang = newLanguage;
      
      if (newLanguage === 'jp' || newLanguage === 'kr' || newLanguage === 'la') {
          print.setFont('./fonts/Pretendard-Regular.ttf');
      } else if (newLanguage === 'ch') {
          print.setFont('./fonts/FengGuangMingRui-Regular.ttf');
      } else if (newLanguage === 'in') {
          print.setFont('./fonts/NotoSans-Regular.ttf');
      }
  
      this.worksheets[0].setData(dataExamples[newLanguage])
    }
}
```
 
