# callbacks for FiveM
Simple server and client callback examples.
<br>Not really a *resource* since it's only ~20 rows of code. I recommend you to add the code to your own project instead.

## Examples
If you run the callbacks on startup you need to add a setTimeout since it takes ~2ms for the exports to actually export. If you don't want to send any data in your callback just simply send an empty object.
### Server callback
```javascript
// # CLIENT
exports['callbacks'].serverCallback('server:testcallback', { 
	content: 'Server callback test' 
}, ( cb ) => {
	console.log('Got a server callback: ', cb);
});

// # SERVER
RegisterNetEvent('server:testcallback')
onNet('server:testcallback', ( data ) => {
    emitNet('callbacks:client', source, data.content, data.CallbackID);
});
```

### Client callback
You probably wanna wrap this in a event or something if you want to send the callback to a specific player.
```javascript
// # CLIENT
RegisterNetEvent('client:testcallback');
onNet('client:testcallback', ( data ) => {
    emitNet('callbacks:server', data.content, data.CallbackID);
});

// # SERVER
exports['callbacks'].clientCallback('client:testcallback', GetPlayerFromIndex(0), { 
  	content: 'Client callback test' 
}, ( cb ) => {
	console.log('Got a client callback: ', cb);
});
```

### Error: No callback ID set
If you get this error it means that you forgot to add the `data.CallbackID`in the `emitNet` function. See the examples above. 

