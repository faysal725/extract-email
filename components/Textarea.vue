<script setup>

const props = defineProps({
    modelValue: [String, Number],
    
    })

    
const inputText = ref("");

const emit = defineEmits(["update:modelValue"]);

watch(
  () => inputText,
  () => {
    emit("update:modelValue", inputText.value);
  },
  { deep: true }
);
</script>

<template>
  <div class="w-10/12 mx-auto">
    <label for="comment" class="block text-sm/6 font-medium text-gray-900">Add your comment</label>
    <div class="mt-2">
      <textarea rows="4" name="comment" id="comment" class="block w-full h-52 rounded-md border-0 p-5 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm/6" 
      v-model="inputText" />
    </div>
    <!-- <button @click="emit('update:modelValue', inputText.value)" type="button" class="rounded bg-indigo-600 px-2 py-1 text-xs font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Convert</button> -->

  </div>
</template>
