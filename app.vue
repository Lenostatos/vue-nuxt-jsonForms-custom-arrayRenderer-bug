<script setup lang="ts">
import { JsonForms } from "@jsonforms/vue";
import { vanillaRenderers } from "@jsonforms/vue-vanilla";
import CustomArrayRendererBuggy from "~/components/CustomArrayRendererBuggy.vue";
import CustomArrayRendererWorkaround from "~/components/CustomArrayRendererWorkaround.vue";
import { rankWith, isPrimitiveArrayControl } from "@jsonforms/core";

// simple data and schema with just one array
let data = { primitiveArray: ["foo", "bar"] };
let schema = {
  $schema: "http://json-schema.org/draft-07/schema#",
  type: "object",
  properties: {
    primitiveArray: {
      type: "array",
      minItems: 1,
      items: {
        type: "string",
      },
    },
  },
};

// Two different sets of renderers
const renderersBuggy = Object.freeze([
  ...vanillaRenderers,
  {
    renderer: CustomArrayRendererBuggy,
    tester: rankWith(4, isPrimitiveArrayControl),
  },
]);
const renderersWorkaround = Object.freeze([
  ...vanillaRenderers,
  {
    renderer: CustomArrayRendererWorkaround,
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
    :renderers="renderersBuggy"
    @change="onChange"
  />
  <h1>Workaround:</h1>
  <JsonForms
    :data="data"
    :schema="schema"
    :renderers="renderersWorkaround"
    @change="onChange"
  />
</template>
