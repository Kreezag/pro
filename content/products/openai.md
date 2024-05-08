![OpenAI Data Grid](img/data-grid/search-and-replace.svg){.icon}

# OpenAI Data Grid

Integrate the intelligence of OpenAI's ChatGPT into your Jspreadsheet Pro with the OpenAI Data Grid extension. This tool seamlessly embeds AI capabilities into your data grid, allowing for automated content generation, response handling, and direct querying about your data. It leverages the OpenAI ChatGPT to deliver a more interactive data exploration and management experience. 

## Endless possibilities

### Generate Marketing Copy

You have a list of products and their attributes, and you need to quickly generate marketing copy for each one. 

```
A1: "Product Name"
B1: "Product Description"
C1: "Marketing Copy"
A2: "Smartwatch"
B2: "Fitness tracking, sleep monitoring, 48-hour battery life"

In C2, you would enter:

=OPENAI.PROMPT("Write an engaging marketing copy for a product named ", A2, " with these features: ", B2)
```
  

## Documentation

### Methods

| Method                    | Description                                                                                                                                                                                                      |
| --------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| =OPENAI.PROMPT(...PROMPT) | This function is used to create dynamic prompts by combining different values or text fragments. It allows you to interact with AI models using customized prompts.                                              |
| =OPENAI.TAG(LIST, OPT)    | This function is used for tasks such as text classification, sentiment analysis, or content filtering. It helps assign relevant tags or categories to input data by leveraging AI models and predefined options. |

 

## Installation

Please choose one of the following options 

### Using NPM

```bash
$ npm install @jspreadsheet/openai
```
 

### Using a CDN

{.ignore}
```html
<script src="https://cdn.jsdelivr.net/npm/@jspreadsheet/openai/dist/index.min.js"></script>
```
 

## Configuration

To incorporate OpenAI into Jspreadsheet, you must include it in the formulas configuration by utilizing the "setFormula" function. 

### Setting as Formula

When setting it as a formula, the OpenAI key should be provided in the configuration object, as demonstrated in the following code snippet: 

{.ignore}
```javascript
import openai from "@jspreadsheet/openai"
import formula from "@jspreadsheet/formula-pro"

formula.setFormula({ OPENAI: openai({ key: '<your-openai-key>' }) })
```
 

## More Examples

### Translating Column to French

This example demonstrates the utilization of OPENAI.PROMPT to translate an entire column effortlessly. 

{.ignore}
```html
<script>
jspreadsheet(dom, {
    data:[
        ['Hello', '=OPENAI.PROMPT("Translate ",A1," to French")'],
        ['Bye'],
        ['Thanks'],
        ['Sorry'],
]})
</script>
```
 

### Combining Words Semantics

 This example demonstrates the utilization of OPENAI.PROMPT to effectively combine word semantics. 

{.ignore}
```html
<script>
jspreadsheet(dom, {
    data:[
            ['Flower', 'Bee', `=OPENAI.PROMPT(A1," and ",B1," combined result in this word:")`],
            ['Fire', 'Water'],
            ['Fire', 'Wood'],
            ['Flower', 'Water'],
            ['Paper', 'Pen'],
            ['Moon', 'Sun']
]})
</script>
```
 

### Classifying Customer Reviews

 This example illustrates the utilization of OPENAI.TAG to assess the intent of customer review messages. 

{.ignore}
```html
<script>
jspreadsheet(self.jssRef, {
    data:[
            ['Reviews', 'Classification', 'Options'],
            ['Very good product', `=OPENAI.TAG(A2:A6, C2:C5)`, 'Positive'],
            ['Very bad product', '', 'Negative'],
            ['Would not buy again', '', 'Neutral'],
            ['I recommend to everyone!'],
            ['Its ok']
]})
</script>
```
 
