# ex-org-tree

> A simple organization tree chart based on Vue2.x

## API

  * #### props


	prop           | descripton                   | type                   | default
	---------------|------------------------------|:----------------------:|---------------------
	data           |                              | `Object`               | 
	props          | configure props              | `Object`               | `{label: 'label', children: 'children', expand: 'expand'}`
	labelWidth     | node label width             | `String` \| `Number`.  | `auto` 
	collapsible    | children node is collapsible | `Boolean`              | `true`
	labelClassName | node label class             | `Function` \| `String` |     -


  * ### events

    - expand-click(status='expanded|collapsed', data)
       
      well be called when the collapse-btn clicked
       

    - node-click(data)
  
      well be called when the node-label clicked

  * ### slot
  
  ```html
    <template slot-scope="scope">
      {{scope.data.label}}
    </template>
  ```


## Example

- default

  ![default](./images/default.png)

- horizontal

  ![horizontal](./images/horizontal.png)

## Browser support

    use table layout!

> IE9+、Chrome、Firefox、Opera



## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
