## ngAppbaseRef API Docs

### $bindEdges()
```js
ngAbRef.$bindEdges(bindOutV, bindSecondLevelEdges, {
	onAdded: function(edgeData, edgeRef, done) {
	},
	onRemoved: function(edgeData, edgeRef, done) {
	},
	onUnbind: function(edgeData, edgeRef) {
	}
})
```
### Returns
An __array__ of edges, _sorted_ by the __priority__ of edges.
Each object (called _edgeData_ from now on) in array represents an edge and contains this data:
```json
{   
    name: (string) edge's name
    priority: (number) edge's priority
    properties: (object) properties of the out-vertex
    edges: (array) (optional) edges of the out-vertex
}
```

### Arguments
- __bindOutV__ _(boolean)_: whether to keep listening to the properties of the out-vertex. If set `false` (default), it will get the properties once and turn off the listener, otherwise, it will keep listening for changes in the properties and they will be modified in the scope variable, too.
- __bindSecondLevelEdges__ _(boolean)_: whether to bind the edges of the out-vertex. Default is `false`, and in the _edgeData_, `edges` won't be available. If set `true`, the edges of the out-vertex can be accessed from `edgeData.edges`- which again is an array of _edgeData_ objects.
- __onAdded__ _(function)_: called when an edge is added. This function basically gives the programmer a facility to modify what should be available in `edgeData`. You can add more data into it, which will be automatically added in to the array, which bound to a scope variable, you don't need to manually add any data to the scope.  The `edgeRef` passed in the callback helps fetching more data relevant to the edge. `done()` should be called when the required data is added in `edgeData`, only after that the `edgeData` will added in the array.
- __onRemoved__ _(function)_: called when an edge is removed. To remove the `edgeData` from the array, calling `done()` is necessary.
- __onUnbind__ _(function)_: called when the scope variable is unbound from edges. You should turn off the listeners here, if added any in the `onAdded` callback.


### $bindProperties()
```js
ngAbRef.$bindProperties({
	onProperties: function(properties, ref, done) {
	},
	onUnbind: function(properties, ref) {
	}
})
```
### Returns
An __object__ , containing _properties_ of the vertex as key-value pairs.

### Arguments
- __varName__ _(string)_: the name of the scope variable, to which the properties are bound
- __onProperties__ _(function)_: called when the properties are received/updated from server 
- __onUnbind__ _(function)_: called when the scope variable is unbound. You should turn off the listeners here, if added any in the `onProperties` callback.