Simple tools for working with JSON.

Contains two actions: Stringify and Parse. Both actions take the input fields, stringify them, and return them. 

This is especially useful for line_items. It also makes it a lot easier to get blocks of information into the Code for Zapier action. 

If you want to return an array, you can use the Code for Zapier action to return an array. Returning an array will cause the subsequent actions to be repeated for each item in the array. 

No external API calls are made. The action simply calls JSON.stringify or JSON.parse in its runtime and returns immediately. 

The code below is the entire code block for the API "call".

### Stringify
Each key in the dictionary is returned as a field with the value stringified.  This is the code that runs.

```js
let line_items = bundle.inputData.line_items 
  ? bundle.inputData.line_items.map(e => e.line_items_dict)
  : undefined;
return Promise.resolve({
  dictionary: z.JSON.stringify(bundle.inputData.dictionary || {}),
  line_items: z.JSON.stringify(line_items || [])
});
```

### Parse
Each key in the dictionary is returned as a field with the value parsed. If a value does not parse, it is set to null, which is also a valid JSON value.

This is the code that runs.

```js
const copy = (d,r) => { 
  Object.keys(d).forEach(e => {
    try {r[e] = JSON.parse(d[e])} catch (x) {r[e] = null} 
  });
};

const output = { 
  dictionary: {}, 
  line_items: []
};
if(bundle.inputData.dict){
  copy(bundle.inputData.dict, output.dictionary);
}
if(Array.isArray(bundle.inputData.line_items)){
  bundle.inputData.line_items.forEach(e => {
    let obj = {}
    copy(e.line_items_dict, obj);
    output.line_items.push(obj)
  })
}
return Promise.resolve({
  dictionary: output.dictionary, 
  line_items: output.line_items
});
```


