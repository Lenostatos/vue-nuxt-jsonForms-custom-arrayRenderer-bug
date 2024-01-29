<script lang="ts" setup>
import type { ControlElement } from '@jsonforms/core';
import { rendererProps, useJsonFormsControl } from '@jsonforms/vue';

// Get JsonForms props
const props = defineProps({
  ...rendererProps<ControlElement>(),
});
// Get JsonForms state and udpate functions 
// (analogue to example in documentation: https://jsonforms.io/api/vue/#md:basic-control-renderer-example)
const jsonForms = useJsonFormsControl(props);

// Bind local variable to input field
const input = ref('baz');

// Define some modification functions using the JsonForms functions
const addNewItem = () => {
  console.log(
    'JsonForms state before overwriting items: ',
    jsonForms.control.value
  );

  const newItems = (jsonForms.control.value.data as Array<any>).concat(
    input.value
  );
  console.log('Items for overwriting: ', newItems);

  jsonForms.handleChange(props.path, newItems);
  console.log(
    'JsonForms state after overwriting items: ',
    jsonForms.control.value
  );
};
</script>

<template>
  <div>
    <select multiple>
      <option v-for="(v, i) in jsonForms.control.value.data" :key="i">
        {{ v }}
      </option>
    </select>
    <br />
    <input v-model="input" />
    <button @click="addNewItem">addNewItem</button>
  </div>
</template>
