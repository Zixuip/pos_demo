<template>
  <div id="app">
    <leftNav></leftNav>
    <router-view />
  </div>
</template>

<script>
import leftNav from "@/components/common/leftNav";
export default {
  name: "App",
  components: {
    leftNav
  }
};
</script>

<style>
.main {
  float: left;
  width: 95%;
  background-color: #eff2f7;
  height: 100%;
  overflow: hidden;
}
</style>
