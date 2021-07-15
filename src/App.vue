<template>
  <div class="application">
    <my-header />
    <main class="main">
      <my-sidebar />
      <my-content />
    </main>
    <my-footer />
  </div>
</template>

<script>
import MyContent from "./components/myContent.vue";
import MyFooter from "./components/myFooter.vue";
import MyHeader from "./components/myHeader.vue";
import MySidebar from "./components/mySidebar.vue";

export default {
  name: "App",
  components: { MyHeader, MySidebar, MyContent, MyFooter },
};
</script>

<style>
@import url("assets/css/style.css");

.application {
  display: grid;
  grid-template-rows: 128px 1fr 128px;
  min-height: 100vh;
}
</style>
