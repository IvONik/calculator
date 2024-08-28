<template>
  <HelloWorld />

  <!-- пропсики -->
  <!-- <Input @addList="adding" />
  <ListComp :arr="arr" /> -->
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import ListComp from "./components/ListComp.vue";
import Input from "./components/Input.vue";

export default {
  data() {
    return {
      arr: [
        {
          title: "rfer",
          body: "rerwer",
        },
      ],
    };
  },
  methods: {
    adding(obj) {
      this.arr.push(obj);
      console.log(this.arr);
    },
  },
  name: "App",
  components: {
    HelloWorld,
    ListComp,
    Input,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}
</style>
