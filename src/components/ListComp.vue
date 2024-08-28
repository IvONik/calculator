<template>
  <div v-for="(item, index) in arr" :key="index">
    <div>Title: {{ item.title }} Body: {{ item.body }}</div>
  </div>
</template>

<script>
export default {
  props: {
    arr: Array,
  },
};
</script>
