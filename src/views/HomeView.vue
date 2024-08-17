

<script setup>
import DynamicForm from "@/components/DynamicForm.vue";
import { ref } from "vue";

const form = ref({});

const fields = ref([
  {
    label: "Activity name",
    prop: "name",
    widget: "input",
    placeholder: "please input activity name",
  },
  {
    label: "Activity zone",
    prop: "region",
    widget: "textarea",
    placeholder: "Activity zone",
  },
  {
    label: "Activity time",
    widget: "dateRange",
    prop: "date1",
    placeholder: "please select your zone",
    props: {
      options: [
        { label: "Zone one", value: "shanghai" },
        { label: "Zone two", value: "beijing" },
      ],
    },
  },
  {
    label: "Instant delivery",
    prop: "delivery",
    widget: "switch",
  },
  {
    label: "Activity type",
    prop: "type",
    widget: "checkboxes",
  },
  {
    label: "Resources",
    prop: "resource",
    widget: "radio",
  },
  {
    label: "Activity form",
    prop: "desc",
    widget: "textarea",
  },
]);
</script>
<template>
  父组件：{{ form }}
  <DynamicForm v-model="form" :value="form" :fields="fields"></DynamicForm>
</template>