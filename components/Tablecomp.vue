<script setup>
const props = defineProps({
  modelValue: [String, Number],
  tabledata: {
    type: Array,
    default: ["asdfsdf"],
  },
});



</script>

<template>

    <div>
        {{ props.tableData }}
    </div>

</template>
