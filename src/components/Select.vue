<template>
  <select :value="modelValue" @change="$emit('update:modelValue', $event.target.value)">
    <option v-if="placeholder" disabled value="">{{ placeholder }}</option>

    <option
        v-for="opt in options"
        :key="opt.value"
        :value="opt.value"
        :disabled="opt.disabled"
    >
      {{ opt.label }}
    </option>
  </select>
</template>

<script setup>
defineProps({
  modelValue: { type: [String, Number], default: "" },
  options: { type: Array, required: true },
  placeholder: { type: String, default: "" },
})
defineEmits(["update:modelValue"])
</script>