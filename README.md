# vue-limit
Vue simple array pagination, fully customizable
[Live Demo](http://marcodpt.github.io/vue-limit)

## Install
```
npm install --save vue-limit
```

## Usage
```javascript
  import Vue from '../node_modules/vue/dist/vue.js'
  import limit from './index.vue'

  new Vue({
    components: {
      'vue-limit': limit
    },
    data: {
      model: {
        page: 1,
        time: 0,
        rows: 3,
        label: 'Page ({{page}} / {{pages}})',
        firstLabel: 'First',
        previousLabel: 'Previous',
        nextLabel: 'Next',
        lastLabel: 'Last'
      },
      input: [
        'item 1',
        'item 2',
        'item 3',
        'item 4',
        'item 5',
        'item 6',
        'item 7',
        'item 8',
        'item 9',
        'item 10'
      ],
      output: []
    }
  }).$mount('#app')
```

```html
  <div style="margin:20px">
    <vue-limit v-bind="model" :model="model" :input="input" :output="output" />
  </div>
  <div style="margin:20px">
    <ul>
      <li v-for="out in output">{{out}}</li>
    </ul>
  </div>
```

### Props
 - model 
   - type: Object
   - required: true
   - description: Object model
 - id
   - type: String
   - default: page
   - description: variable id inside object
 - rows
   - type: Number
   - default: 10
   - description: max number of elements of **input** array per page
 - input
   - type: Array
   - default: []
   - description: Input data, component don't touch in this array, **read only**
 - output
   - type: Array
   - default: []
   - description: You should pass an empty array inside your data scope, this array will be stored result of pagination, observe that in case of objects exact the same object from input will be passed
 - label
   - type: String
   - default: Page (**{{page}}** / **{{pages}}**)
   - description: String that will be used on select, **page** and **pages** are the only available variables
 - time
   - type: Number
   - default: 0
   - description: 0 does nothing, greater than 0 means time in second to go automatically to next page
 - firstLabel
   - type: String
   - default: First
   - description: First button label
 - previousLabel
   - type: String
   - default: Previous
   - description: Previous button label
 - nextLabel
   - type: String
   - default: Next
   - description: Next button label
 - lastLabel
   - type: String
   - default: Last
   - description: Last button label
 - buttonClass
   - type: String, Array, Object
   - default: ''
   - description: CSS class aplied to all the buttons, for example with bootstrap3 use **btn btn-default**
 - buttonStyle
   - type: String, Array, Object
   - default: ''
   - description: CSS inline style for all buttons
 - selectClass
   - type: String, Array, Object
   - default: ''
   - description: CSS class aplied to select, for example with bootstrap3 use **form-control**
 - selectStyle
   - type: String, Array, Object
   - default: ''
   - description: CSS inline style to select, for example with bootstrap3 use **width:auto;display:inline**

### Slots
 - slots inside the buttons, for use icons instead of labels: 
   - first
   - previous
   - next
   - last
 - slot for substitute the select input, use this slot if you want a simple **label** or something like **input=range**
   - select
     - scope
       - id: id passed in the props
       - model: model passed in the props
       - label: current label, the same passed in the props but with variable substitution
       - options: **array** with **{id, label}** that generate select

    As you can see you can be use any framework you like with very good results  
    And you can be very creative for substitute the select for other thing

## Contribute
We need help! Our goals are:
 - Support an easy and quick api interface for array pagination
 - Add tests
 - More usage examples and better home page
 - Add support to most browsers

What is outside of the scope of this project:
 - Use any kind of css framework or any layout decision
