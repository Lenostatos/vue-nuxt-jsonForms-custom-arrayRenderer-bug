<script lang="ts" setup>
import type { ControlElement } from '@jsonforms/core';
import { rendererProps, useJsonFormsArrayControl } from '@jsonforms/vue';

// Get props, control, and udpate functions from JsonForms (analogue to example in documentation: https://jsonforms.io/api/vue/#md:basic-control-renderer-example)
const props = defineProps({
  ...rendererProps<ControlElement>(),
});
const { control, addItem, removeItems, moveDown, moveUp } =
  useJsonFormsArrayControl(props);

// Bind local variable to input field
const input = ref('baz');

// Define some modification functions using the JsonForms functions
// !!! These all throw errors (see console) !!!
const addNewItem = () => {
  console.log('JsonForms state before adding item: ', control.value);

  const newItem = input.value;
  console.log('Item to be added: ', newItem);

  addItem(props.path, newItem)();
  console.log('JsonForms state after adding item: ', control.value);
};

const removeFirstValue = () => {
  if (removeItems) {
    removeItems(props.path, [0])();
  } else {
    console.error("removeItems function was not provided by JsonForms API");
  }
};

const moveDownFirstValue = () => {
  if (moveDown){
    moveDown(props.path, 0)();
  } else {
    console.error("moveDown function was not provided by JsonForms API");
  }
};

const moveUpSecondValue = () => {
  if (moveUp){
    moveUp(props.path, 1)();
  } else {
    console.error("moveUp function was not provided by JsonForms API");
  }
};
</script>

<template>
  <div>
    <select multiple>
      <option v-for="(v, i) in control.data" :key="i">
        {{ v }}
      </option>
    </select>
    <br />
    <input v-model="input" />
    <button @click="addNewItem">addNewItem</button>
    <br />
    <button @click="removeFirstValue">removeFirstValue</button>
    <br />
    <button @click="moveDownFirstValue">moveDownFirstValue</button>
    <br />
    <button @click="moveUpSecondValue">moveUpSecondValue</button>
  </div>
</template>
