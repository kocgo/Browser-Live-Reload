## Server Setup
_Requirements_
```js
const express = require("express");
const app = express();
const port = 3000;
const socket_port = 8080;
const io = require('socket.io').listen(socket_port);
const fs = require('fs');
```

_Static files for front-end_
```js
app.use('/node_modules/socket.io-client/dist/',  express.static(__dirname + '/node_modules/socket.io-client/dist'));
```

_Filesysem module watches the current directory recursively and if there is any change callback is being executed_
```js
fs.watch(__dirname, { recursive:true }, function () {
  setTimeout(() => io.emit('file-change-event'), 1000);
});
```

## Client (Front-end) Setup
```html
<script src="/node_modules/socket.io-client/dist/socket.io.js"></script>
    <script>
        const socket = io.connect('http://localhost:8080');
        socket.on("file-change-event", function () {
            window.location.reload();
        });
    </script>
```
