<template>
  <button :class="className" @click="$emit('click', $event)"
                :type="type">
    {{text}}
  </button>
</template>

<script setup>

defineProps({
  type: { type: String, default: "button" },
  text: { type: String, default: "" },
  className: {type: String, default: ""}
})

defineEmits(["click"])

</script>