<template>
  <my-map />
</template>

<script>
import MyMap from "./components/MyMap";

export default {
  name: "App",
  components: {
    MyMap
  }
};
</script>

<style>
html,
body,
#app,
div.container {
  height: 100%;
  margin: 0;
}
</style>
