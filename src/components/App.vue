<template>
  <div class='container-fluid bg-picture pt-3'>
    <h1 class="text-light text-center">Poke Dragger</h1>
    <the-icon-list></the-icon-list>
  </div>
</template>

<script>
  import {defineComponent} from "vue";
  import TheIconList from "./TheIconList.vue";

  const App = defineComponent({
    name: "App",
    components: {
      TheIconList
    },
    setup() {
      return {

      }
    },
  })
  export default App
</script>

<style>
.bg-picture { 
  background: url(../assets/bg.png) no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  height: 100vh;
}
</style>