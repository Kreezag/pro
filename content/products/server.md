title: Spreadsheet Real-time Collaboration with Jspreadsheet Server
keywords: Jspreadsheet, Jexcel, javascript, grid, data grid, real-time spreadsheet collaboration, Jspreadsheet Server, data persistence, real-time collaboration, JavaScript plugin, data management, data grid plugin
description: Jspreadsheet Server is a powerful product designed to facilitate data persistence and enable real-time collaboration.

![Data Grid Server](img/data-grid/server.svg){.icon}

# Jspreadsheet Server

The Jspreadsheet Server extension is a JavaScript plugin designed for real-time data sharing and persistence in Jspreadsheet. It is a web-socket-based service hosted on your server that allows collaboration and interactivity within spreadsheets while your data is 100% under your control.

## Highlights

* **Private Service**: Host on your servers for total data privacy;
* **Custom Authentication**: Use your authentication methods;
* **Custom Storage**: Add your persistence mechanisms;
* **Real-time Collaboration**: Work together on spreadsheets in real time;
* **Lightweight**: Experience seamless use with intuitive controls;
* **WebSockets**: Ensure smooth communication with Jspreadsheet;


{.green}
> Jspreadsheet Server enhances your applications with real-time collaboration and data persistence securely hosted on your servers.  

## Documentation

The Jspreadsheet Server maintains a remote spreadsheet configuration for sharing across different users.  

### How to install

```bash
npm install @jspreadsheet/server
```

### Settings

| Property                                               | Description                                             |
|--------------------------------------------------------|---------------------------------------------------------|
| port: string                                           | Service Port. Default: 3000                             |
| license: object                                        | License information                                     |
| auth: async function(socket) => boolean                | User authentication handler.                            |
| load: async function(string) => object                 | Load the spreadsheet configuration by guid.             |
| create: async function(config) => object               | Create a new spreadsheet with the given configuration.  |
| change: async function(config) => object               | Update a given spreadsheet configuration.               |
| destroy: async function(string) => boolean             | Delete a spreadsheet by guid.                           |
| connect: async function(socket, identifier) => void    | It is triggered when a new user joins.                  |
| disconnect: async function(socket, identifier) => void | Disconnect event                                        |
| error: async function(e) => void                       | Something went wrong and why.                           |

## Development considerations

Jspreadsheet Server is really flexible and requires you to declare some features such as authentication and some persistent events. So, we the following table gives you some considerations for each available event.

### Auth
This event can be used to ensure that users have appropriate access to the spreadsheets. You can use `socket.query.token` to retrieve the token presented on the frontend.

### Change
In this event, developers can access and possibly update the spreadsheet's configuration. However, updating the entire configuration may need to be more efficient, as demonstrated in the example below. The information presented in the event can be used to optimize the persistence methods.

### Connect
Allows the addition of custom events to the socket, enhancing interaction capabilities within the Jspreadsheet environment.

### Error
Allows the developer to save the error in a file for debug purposes for example.

{.green}
> **Server Monitoring**
> Employing tools like `pm2` or `supervisor` is essential for server monitoring. These tools automate process management, ensuring your server remains operational and enhances deployment reliability.  

## Basic template

### Create a Server With Persistence

This template demonstrates setting up a Jspreadsheet server for collaborative editing and persistence, detailing server initialization, user authentication, and spreadsheet event handling functions.

{.ignore}
```javascript
const server = require('@jspreadsheet/server');

// Jspreadsheet license: Both available on your profile
const license = {
    clientId: 'your-client-id',
    licenseKey: 'your-certificate-license'
}

// Create jspreadsheet server
server({
    port: 3000,
    config: {
        // Socket IO server configuration
    },
    error: async function(e) {
        // Save the error in a file  
    },
    auth: async function(socket) {
        // Return true when the user is authenticated
        return true;
    },
    load: async function(guid) {
        // Load an existing spredasheet based on the guid identifier
    },
    create: async function(guid, config) {
        // Create a new spreadsheet
    },
    destroy: async function(guid) {
        // Destroy an existing spreadsheet
    },
    change: async function(guid, changes) {
        // Update an existing spradsheet
    },
    connect: async function(socket, ident) {
        // When a new user connects
    },
    disconnect: async function(socket, ident) {
        // When a user disconnects
    },
    license: license,
});

```

## Example

### Saving the data with Redis

#### Server Side

This example a basic data persistence implementation using Redis on the server side without access restrictions.

{.ignore}
```javascript
const server = require('@jspreadsheet/server');
const { createClient } = require("redis");

const client = createClient({
    socket: {
        host: 'redis',
        port: 6379
    },
});

// Connect to the server
client.connect();

// Jspreadsheet license
const license = {
    clientId: '356a192b7913b04c54574d18c28d46e6395428ab',
    licenseKey: 'MmIyMDhmYmY4NGI1ZDY1ODAwNThjMGZkOTVkNjg2MmQ1NzZmYTFhOTBmZWI3N2M3ZmQ1N2Q3YjMwNDNhMjRhYmViYmRkNGVjZjZlMmNkNDVhODJhYzg1ZmRiY2E3OTJhYjA1ODQzNTliZGZiMmYwNWM4YmRmMjAyZmUwODA1NmEsZXlKamJHbGxiblJKWkNJNklqTTFObUV4T1RKaU56a3hNMkl3TkdNMU5EVTNOR1F4T0dNeU9HUTBObVUyTXprMU5ESTRZV0lpTENKdVlXMWxJam9pU25Od2NtVmhaSE5vWldWMElpd2laR0YwWlNJNk1UYzBNak0wTWpRd01Dd2laRzl0WVdsdUlqcGJJbXB6YUdWc2JDNXVaWFFpTENKamMySXVZWEJ3SWl3aWFuTndjbVZoWkhOb1pXVjBMbU52YlNJc0luVmxMbU52YlM1aWNpSXNJbU5rY0c0dWFXOGlMQ0pwYm5SeVlYTm9aV1YwY3k1amIyMGlMQ0p6Wm1OdlpHVmliM1F1WTI5dElpd2lkMlZpSWl3aWJHOWpZV3hvYjNOMElsMHNJbkJzWVc0aU9pSXpOQ0lzSW5OamIzQmxJanBiSW5ZM0lpd2lkamdpTENKMk9TSXNJbll4TUNJc0luWXhNU0lzSW1admNtMXpJaXdpWm05eWJYVnNZU0lzSW5KbGJtUmxjaUlzSW5CaGNuTmxjaUlzSW1sdGNHOXlkR1Z5SWl3aWRtRnNhV1JoZEdsdmJuTWlMQ0pqYjIxdFpXNTBjeUlzSW5ObFlYSmphQ0lzSW1Ob1lYSjBjeUlzSW5CeWFXNTBJaXdpWW1GeUlpd2ljMmhsWlhSeklpd2lZMnh2ZFdRaUxDSnRZWE5ySWl3aWMyaGxaWFJ6SWl3aWMyVnlkbVZ5SWl3aWFXNTBjbUZ6YUdWbGRITWlYWDA9'
}

server({
    port: 3000,
    // Socker.io server configuration
    config: {
        cors: {
            origin: "*"
        },
    },
    error: async function(e) {
        console.log(e);
        // Kill the thread
        process.exit(1);
    },
    auth: async function(socket) {
        return true;
    },
    load: async function(guid) {
        return await client.get(guid);
    },
    create: async function(guid, config) {
        const result = await client.exists(guid);

        if (result) {
            // A spreadsheet already exists
            return false;
        } else {
            // Create a new spreadsheet
            await client.set(guid, config);

            return true;
        }
    },
    destroy: async function(guid) {
        return await client.del(guid)
            .then(() => true)
            .catch(() => false);
    },
    change: async function(guid, changes) {
        // Get the configuration from the cache
        let config = changes.instance.getConfig();
        // Save that on the redis
        await client.set(guid, JSON.stringify(config));
    },
    license: license,
});
```

#### Client

Connect to your server and create and open an existing remote spreadsheet using the spreadsheet guid ident.

```html
<html>
<link rel="stylesheet" href="https://jspreadsheet.com/v11/jspreadsheet.css" type="text/css" />
<link rel="stylesheet" href="https://jsuites.net/v5/jsuites.css" type="text/css" />
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Material+Icons" />

<script src="https://jspreadsheet.com/v11/jspreadsheet.js"></script>
<script src="https://jsuites.net/v5/jsuites.js"></script>
<script src="https://cdn.socket.io/4.3.2/socket.io.min.js"></script>

<div id="spreadsheet"></div>

<script>
jspreadsheet.setExtensions({ formula, client });

// Connect
let remote = client.connect({
    // Point this to your own domain server
    url: 'https://jspreadsheet.com',
    // Internal socket path
    path: 'server/socket.io/',
    // Can be used to send extra information to the server to validate this user connection
    token: 'user-identifier'
});

// Create in case does not exist
remote.create('53aa4c90-791d-4a65-84a6-8ac25d6b1105', {
    tabs: true,
    toolbar: true,
    worksheets: [{
        minDimensions: [4,6]
    }]
});

// Connect to a spreadsheet
jspreadsheet(document.getElementById('spreadsheet'), {
    guid: '53aa4c90-791d-4a65-84a6-8ac25d6b1105'
});
</script>
```
```jsx
import React, {useRef} from 'react';
import { Spreadsheet, jspreadsheet } from '@jspreadsheet/react';
import formula from '@jspreadsheet/formula-pro';
import client from '@jspreadsheet/client';

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = {
  clientId: '356a192b7913b04c54574d18c28d46e6395428ab',
  licenseKey: 'MmIyMDhmYmY4NGI1ZDY1ODAwNThjMGZkOTVkNjg2MmQ1NzZmYTFhOTBmZWI3N2M3ZmQ1N2Q3YjMwNDNhMjRhYmViYmRkNGVjZjZlMmNkNDVhODJhYzg1ZmRiY2E3OTJhYjA1ODQzNTliZGZiMmYwNWM4YmRmMjAyZmUwODA1NmEsZXlKamJHbGxiblJKWkNJNklqTTFObUV4T1RKaU56a3hNMkl3TkdNMU5EVTNOR1F4T0dNeU9HUTBObVUyTXprMU5ESTRZV0lpTENKdVlXMWxJam9pU25Od2NtVmhaSE5vWldWMElpd2laR0YwWlNJNk1UYzBNak0wTWpRd01Dd2laRzl0WVdsdUlqcGJJbXB6YUdWc2JDNXVaWFFpTENKamMySXVZWEJ3SWl3aWFuTndjbVZoWkhOb1pXVjBMbU52YlNJc0luVmxMbU52YlM1aWNpSXNJbU5rY0c0dWFXOGlMQ0pwYm5SeVlYTm9aV1YwY3k1amIyMGlMQ0p6Wm1OdlpHVmliM1F1WTI5dElpd2lkMlZpSWl3aWJHOWpZV3hvYjNOMElsMHNJbkJzWVc0aU9pSXpOQ0lzSW5OamIzQmxJanBiSW5ZM0lpd2lkamdpTENKMk9TSXNJbll4TUNJc0luWXhNU0lzSW1admNtMXpJaXdpWm05eWJYVnNZU0lzSW5KbGJtUmxjaUlzSW5CaGNuTmxjaUlzSW1sdGNHOXlkR1Z5SWl3aWRtRnNhV1JoZEdsdmJuTWlMQ0pqYjIxdFpXNTBjeUlzSW5ObFlYSmphQ0lzSW1Ob1lYSjBjeUlzSW5CeWFXNTBJaXdpWW1GeUlpd2ljMmhsWlhSeklpd2lZMnh2ZFdRaUxDSnRZWE5ySWl3aWMyaGxaWFJ6SWl3aWMyVnlkbVZ5SWl3aWFXNTBjbUZ6YUdWbGRITWlYWDA9'
}

jspreadsheet.setLicense(license);

jspreadsheet.setExtensions({ formula, client });

const guid = '53aa4c90-791d-4a65-84a6-8ac25d6b1105'

// Connect to the server
let remote = client.connect({
    // Point this to your own domain server
    url: 'https://jspreadsheet.com',
    // Internal socket path
    path: 'server/socket.io/',
    // Can be used to send extra information to the server to validate this user connection
    token: 'user-identifier'
});

// Create just one time. Do nothing if already exists
remote.create(guid, {
  tabs: true,
  toolbar: true,
  worksheets: [{
    minDimensions: [4,6]
  }]
}).then(() => {});

export default function App() {
  // Spreadsheet array of worksheets
  const spreadsheet = useRef();

  // Render component
  return (
      <Spreadsheet ref={spreadsheet} guid={guid} />
  );
}
```
```vue
<template>
  <Spreadsheet ref="spreadsheet" :guid="guid"/>
</template>

<script>
import { Spreadsheet, jspreadsheet } from "@jspreadsheet/vue";
import formula from "@jspreadsheet/formula-pro";
import client from "@jspreadsheet/client";

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = {
  clientId: '356a192b7913b04c54574d18c28d46e6395428ab',
  licenseKey: 'MmIyMDhmYmY4NGI1ZDY1ODAwNThjMGZkOTVkNjg2MmQ1NzZmYTFhOTBmZWI3N2M3ZmQ1N2Q3YjMwNDNhMjRhYmViYmRkNGVjZjZlMmNkNDVhODJhYzg1ZmRiY2E3OTJhYjA1ODQzNTliZGZiMmYwNWM4YmRmMjAyZmUwODA1NmEsZXlKamJHbGxiblJKWkNJNklqTTFObUV4T1RKaU56a3hNMkl3TkdNMU5EVTNOR1F4T0dNeU9HUTBObVUyTXprMU5ESTRZV0lpTENKdVlXMWxJam9pU25Od2NtVmhaSE5vWldWMElpd2laR0YwWlNJNk1UYzBNak0wTWpRd01Dd2laRzl0WVdsdUlqcGJJbXB6YUdWc2JDNXVaWFFpTENKamMySXVZWEJ3SWl3aWFuTndjbVZoWkhOb1pXVjBMbU52YlNJc0luVmxMbU52YlM1aWNpSXNJbU5rY0c0dWFXOGlMQ0pwYm5SeVlYTm9aV1YwY3k1amIyMGlMQ0p6Wm1OdlpHVmliM1F1WTI5dElpd2lkMlZpSWl3aWJHOWpZV3hvYjNOMElsMHNJbkJzWVc0aU9pSXpOQ0lzSW5OamIzQmxJanBiSW5ZM0lpd2lkamdpTENKMk9TSXNJbll4TUNJc0luWXhNU0lzSW1admNtMXpJaXdpWm05eWJYVnNZU0lzSW5KbGJtUmxjaUlzSW5CaGNuTmxjaUlzSW1sdGNHOXlkR1Z5SWl3aWRtRnNhV1JoZEdsdmJuTWlMQ0pqYjIxdFpXNTBjeUlzSW5ObFlYSmphQ0lzSW1Ob1lYSjBjeUlzSW5CeWFXNTBJaXdpWW1GeUlpd2ljMmhsWlhSeklpd2lZMnh2ZFdRaUxDSnRZWE5ySWl3aWMyaGxaWFJ6SWl3aWMyVnlkbVZ5SWl3aWFXNTBjbUZ6YUdWbGRITWlYWDA9'
}

jspreadsheet.setLicense(license);

jspreadsheet.setExtensions({ formula, client });

const guid = '53aa4c90-791d-4a65-84a6-8ac25d6b1105'

// Connect to the server
let remote = client.connect({
    // Point this to your own domain server
    url: 'https://jspreadsheet.com',
    // Internal socket path
    path: 'server/socket.io/',
    // Can be used to send extra information to the server to validate this user connection
    token: 'user-identifier'
});

// Create just one time. Do nothing if already exists
remote.create(guid, {
        tabs: true,
        toolbar: true,
        worksheets: [
            {
                minDimensions: [4, 6],
            },
        ],
    })
    .then(() => { });

export default {
  components: {
      Spreadsheet,
  },
  data() {

      return {
          guid
      };
  }
}
</script>
```
```angularjs
import { Component, ViewChild, ElementRef } from "@angular/core";
import jspreadsheet from "jspreadsheet";
import formula from "@jspreadsheet/formula-pro";
import client from "@jspreadsheet/client";

import "jsuites/dist/jsuites.css";
import "jspreadsheet/dist/jspreadsheet.css";

const license = {
  clientId: '356a192b7913b04c54574d18c28d46e6395428ab',
  licenseKey: 'MmIyMDhmYmY4NGI1ZDY1ODAwNThjMGZkOTVkNjg2MmQ1NzZmYTFhOTBmZWI3N2M3ZmQ1N2Q3YjMwNDNhMjRhYmViYmRkNGVjZjZlMmNkNDVhODJhYzg1ZmRiY2E3OTJhYjA1ODQzNTliZGZiMmYwNWM4YmRmMjAyZmUwODA1NmEsZXlKamJHbGxiblJKWkNJNklqTTFObUV4T1RKaU56a3hNMkl3TkdNMU5EVTNOR1F4T0dNeU9HUTBObVUyTXprMU5ESTRZV0lpTENKdVlXMWxJam9pU25Od2NtVmhaSE5vWldWMElpd2laR0YwWlNJNk1UYzBNak0wTWpRd01Dd2laRzl0WVdsdUlqcGJJbXB6YUdWc2JDNXVaWFFpTENKamMySXVZWEJ3SWl3aWFuTndjbVZoWkhOb1pXVjBMbU52YlNJc0luVmxMbU52YlM1aWNpSXNJbU5rY0c0dWFXOGlMQ0pwYm5SeVlYTm9aV1YwY3k1amIyMGlMQ0p6Wm1OdlpHVmliM1F1WTI5dElpd2lkMlZpSWl3aWJHOWpZV3hvYjNOMElsMHNJbkJzWVc0aU9pSXpOQ0lzSW5OamIzQmxJanBiSW5ZM0lpd2lkamdpTENKMk9TSXNJbll4TUNJc0luWXhNU0lzSW1admNtMXpJaXdpWm05eWJYVnNZU0lzSW5KbGJtUmxjaUlzSW5CaGNuTmxjaUlzSW1sdGNHOXlkR1Z5SWl3aWRtRnNhV1JoZEdsdmJuTWlMQ0pqYjIxdFpXNTBjeUlzSW5ObFlYSmphQ0lzSW1Ob1lYSjBjeUlzSW5CeWFXNTBJaXdpWW1GeUlpd2ljMmhsWlhSeklpd2lZMnh2ZFdRaUxDSnRZWE5ySWl3aWMyaGxaWFJ6SWl3aWMyVnlkbVZ5SWl3aWFXNTBjbUZ6YUdWbGRITWlYWDA9'
}

jspreadsheet.setLicense(license);

// Define the data grid extensions
jspreadsheet.setExtensions({ client, formula });

// Connect
let remote = client.connect({
    // Point this to your own domain server
    url: 'https://jspreadsheet.com',
    // Internal socket path
    path: 'server/socket.io/',
    // Can be used to send extra information to the server to validate this user connection
    token: 'user-identifier'
});

// Create in case does not exist
remote.create('53aa4c90-791d-4a65-84a6-8ac25d6b1105', {
    tabs: true,
    toolbar: true,
    worksheets: [{
        minDimensions: [4,6]
    }]
});

@Component({
    selector: "app-root",
    template: `<div #spreadsheet></div>`
})
export class AppComponent {
    @ViewChild("spreadsheet") spreadsheet: ElementRef;
    // Worksheets
    worksheets: jspreadsheet.worksheetInstance[];
    // Create a new data grid
    ngAfterViewInit() {


    // Create spreadsheet
    this.worksheets = jspreadsheet(this.spreadsheet.nativeElement, {
        guid: '53aa4c90-791d-4a65-84a6-8ac25d6b1105'
    });
    }
}
```

## More resources

### Real-time Spreadsheets with React Typescript

Use this React TypeScript project as a template to set up a real-time collaborative spreadsheet server within minutes.

- https://github.com/jspreadsheet/spreadsheet-react-server


### Jspreadsheet Server Nginx Setup

You can quick enable your server using nginx following this tutorial

- https://github.com/jspreadsheet/server