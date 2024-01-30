<script setup lang="ts">
import { JsonForms } from "@jsonforms/vue";
import { vanillaRenderers } from "@jsonforms/vue-vanilla";
import CustomArrayRenderer from "~/components/CustomArrayRenderer.vue";
import { rankWith, isPrimitiveArrayControl } from "@jsonforms/core";

// simple data and schema with just one array
let data = { myPrimitiveArray: ["foo", "bar"] };
let schema = {
  $schema: "http://json-schema.org/draft-07/schema#",
  type: "object",
  properties: {
    myPrimitiveArray: {
      type: "array",
      minItems: 1,
      items: {
        type: "string",
      },
    },
  },
};

// Integrate the custom renderer
const renderers = Object.freeze([
  ...vanillaRenderers,
  {
    renderer: CustomArrayRenderer,
    tester: rankWith(4, isPrimitiveArrayControl),
  },
]);

// Updating and logging form data on change
const onChange = (event: any) => {
  data = event.data;
  console.log("Form data has changed: ", data);
};
</script>

<template>
  <JsonForms
    :data="data"
    :schema="schema"
    :renderers="renderers"
    @change="onChange"
  />
</template>
