<template>
  <h2 :class="className">{{ text }}</h2>
</template>

<script setup>
defineProps({
  text: {
    type: String,
    default: ""
  },
  className: { type: String, default: "" },
})
</script>