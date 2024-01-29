<script lang="ts" setup>
import type { ControlElement } from "@jsonforms/core";
import { rendererProps, useJsonFormsControl } from "@jsonforms/vue";

// Get props, control, and the addItem function from JsonForms
// (analogue to example in documentation:
// https://jsonforms.io/api/vue/#md:basic-control-renderer-example)
const props = defineProps({ ...rendererProps<ControlElement>() });
const jsonForms = useJsonFormsControl(props);

// Define some modification functions using the JsonForms functions
const addItemOnClick = () => {
  console.log(
    "JsonForms state before overwriting items: ",
    jsonForms.control.value
  );

  const newItems = (jsonForms.control.value.data as Array<any>).concat("baz");
  console.log("Items for overwriting: ", newItems);

  jsonForms.handleChange(props.path, newItems);
  console.log(
    "JsonForms state after overwriting items: ",
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
    <button @click="addItemOnClick">add item</button>
  </div>
</template>
