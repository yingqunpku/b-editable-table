# BootstrapVue Editable Table
![Demo](https://github.com/muhimasri/b-editable-table/blob/main/images/demo.gif)

BootstrapVue Editable Table is a Vue table component that enables cell editing and inherits/supports all other features from [Bootstrap Table](https://bootstrap-vue.org/docs/components/table).

**This is still an early stage beta version so please help by creating issues with proper labels (bug, question, enhancement...).** Thank you in advance 🙏

If you'd like to contribute, please read this [introductory article](https://muhimasri.com/blogs/create-an-editable-dynamic-table-with-bootstrap-vue/).

## Prerequisite

A basic understanding of [BootstrapVue Table](https://bootstrap-vue.org/docs/components/table).

You're required to install Bootstrap and Bootstrap Vue in your project:

```
npm install bootstrap bootstrap-vue
```

## Setup
```
npm i bootstrap-vue-editable-table
```

Since this is a BootstrapVue component, you need to set it up the same way. The easiest approach is to register BootstrapVue in your app entry point (typically app.js or main.js):

```javascript
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap and BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
```
Please refer to [BoostrapVue Docs](https://bootstrap-vue.org/docs) for more details.

## Usage

```javascript
<template>
    <b-editable-table :items="items" :fields="fields"></b-editable-table>
</template>

<script>
import BEditableTable from 'vue-bootstrap-editable-table';
export default {
  components: {
    BEditableTable
  },
  data() {
    return {
      fields: [
        { key: "name", label: "Name", type: "text"},
        { key: "department", label: "Department", type: "select", options: ['Marketing', 'Development', 'HR', 'Marketing'] },
        { key: "age", label: "Age", type: "number" },
        { key: "dateOfBirth", label: "Date Of Birth", type: "date" },
        { key: "isActive", label: "Is Active", type: "checkbox" },
      ],
       items: [
          { age: 40, name: 'Dickerson', department: 'Development', dateOfBirth: '1984-05-20', isActive: true },
          { age: 21, name: 'Larsen', department: 'Marketing', dateOfBirth: '1984-05-20', isActive: false },
          { age: 89, name: 'Geneva', department: 'HR', dateOfBirth: '1984-05-20', isActive: false },
          { age: 38, name: 'Jami', department: 'Accounting', dateOfBirth: '1984-05-20', isActive: true }
        ]
    };
  },
};
</script>
```

`items` and `fields` are the same properties used in BootstrapVue Table except we are introducing a new `type` property in the `fields` object to indicate what element is required in every column.

For `select` element, the options can be passed as another property (as shown in the example above). Since this is a [Boostrap Form Select](https://bootstrap-vue.org/docs/components/form-select), it supports a list of strings or key/value objects:

```json
[
  { value: 'a', text: 'First option' },
  { value: 'b', text: 'Second Option' },
  { value: 'b', text: 'Third Option' }
]
```

## Form Elements:
Every column requires a `type` in order to make the cell editable:

```json
[
  { key: "name", label: "Name", type: "text"},
  { key: "department", label: "Department", type: "select", options: ['Marketing', 'Development', 'HR'] },
  { key: "age", label: "Age", type: "number" },
  { key: "dateOfBirth", label: "Date Of Birth", type: "date" },
  { key: "isActive", label: "Is Active", type: "checkbox" },
]
```
#### Below are the supported Bootstrap form elements:
|Type | Description |
|--|--|
| text | Bootstrap Form Text Input
| number | Bootstrap Form Number Input
| select | Bootstrap Form Select
| date | Bootstrap Form Datepicker
| checkbox | Bootstrap Form Checkbox

## Behavior:
Every cell will change to an editable input field upon clicking on the cell. 

#### Supported keyboard keys:
|Key |Behavior |
|--|--|
| Tab | Move to the next cell |
| Esc | Exit edit mode |

## Data Binding:
Editable cells use `v-model` internally to support **two-way data bindings**. Any change will reflect directly on the `items` array.

## Events:
|Event |Arguments | Description |
|--|--|--|
| input-change &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |`value` - Current cell value <br/> `data` - Row data (the same object returned by Bootstrap)| Emitted when any cell input changes

#### Example:
```javascript
<template>
    <b-editable-table :items="items" :fields="fields" @input-change="handleInput"></b-editable-table>
</template>

<script>
import BEditableTable from 'vue-bootstrap-editable-table';
export default {
  components: {
    BEditableTable
  },
  data() {
    return {
      fields: [
        { key: "name", label: "Name", type: "text"},
        { key: "department", label: "Department", type: "select", options: ['Marketing', 'Development', 'HR', 'Accounting'] },
        { key: "age", label: "Age", type: "number" },
        { key: "dateOfBirth", label: "Date Of Birth", type: "date" },
        { key: "isActive", label: "Is Active", type: "checkbox" },
      ],
       items: [
          { age: 40, name: 'Dickerson', department: 'Development', dateOfBirth: '1984-05-20', isActive: true },
          { age: 21, name: 'Larsen', department: 'Marketing', dateOfBirth: '1984-05-20', isActive: false },
          { age: 89, name: 'Geneva', department: 'HR', dateOfBirth: '1984-05-20', isActive: false },
          { age: 38, name: 'Jami', department: 'Accounting', dateOfBirth: '1984-05-20', isActive: true }
        ]
    };
  },
  methods: {
      handleInput(value, data) {
	      // Handle input change
      }
  }
};
</script>
```
## Custom Cell
To customize a none editable cell, you can use scoped slots to customize a particular field.

#### Example rendering a `boolean` field to `Yes` or `No` value:

```javascript
<b-editable-table :items="items" :fields="fields">
	<template #cell-isActive="data">
		<span v-if="data.value">Yes</span>
		<span v-else>No</span>
	</template>
</b-editable-table>
```

The slot name has to start with `cell-` then followed by the field key `cell-isActive`

|Name | Description |
|--|--|--|
| `cell-{key}` &nbsp; &nbsp; &nbsp; &nbsp;| Scoped slot for custom data rendering of field data. '{key}' is the field's key name.

## Styling
There is still no built-in styling or themes for the editable table so input fields will look like the default Bootstrap form elements.

To customize an element, simply add a class to the component and use a CSS selector to style a specific element.

#### Example:
```html
<template>
	<b-editable-table  class="b-table" :items="items" :fields="fields"></b-editable-table>
</template>
<style>
/** Change the border color for the input field */
.b-table input {
    border: 1px solid red;
}
</style>
```

## Supported Version
This has only been tested on:

 - vue `v2.6.12`
 - bootstrap `v5.0.1`
 - bootstrap-vue `v2.21.2`

We are looking to support as many versions as possible but please create an issue if you encounter compatibility issues 🙏