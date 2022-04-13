<template>
  <div v-html="innerHtml"></div>
</template>

<script lang="ts" setup>
defineProps<{ innerHtml: string }>();
</script>
