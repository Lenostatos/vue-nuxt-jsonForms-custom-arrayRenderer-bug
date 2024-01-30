<script lang="ts" setup>
import type { ControlElement } from "@jsonforms/core";
import { rendererProps, useJsonFormsArrayControl } from "@jsonforms/vue";

// Get props, control, and the addItem function from JsonForms
// (analogue to example in documentation:
// https://jsonforms.io/api/vue/#md:basic-control-renderer-example)
const props = defineProps({ ...rendererProps<ControlElement>() });
const { control, addItem } = useJsonFormsArrayControl(props);

// ! This throws the error (see console) !
const addItemBaz = () => {
  console.log("JsonForms state before adding item: ", control.value);
  addItem(props.path, "baz")();
  console.log("JsonForms state after adding item: ", control.value);
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
    <button @click="addItemBaz">add item "baz"</button>
  </div>
</template>
