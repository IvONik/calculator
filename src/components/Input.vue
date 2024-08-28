<template>
  <div>
    <input type="text" v-model="text1" />
    <input type="text" v-model="text2" />
    <button @click="addList">add</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text1: "",
      text2: "",
      post: [],
    };
  },
  methods: {
    addList() {
      const obj = {
        title: this.text1,
        body: this.text2,
      };
      this.$emit("addList", obj);
    },
  },
};
</script>
