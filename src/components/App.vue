<template>
  <div class='container-fluid bg-picture pt-3 h-100'>
    <h1 class="text-light ps-3">PokeDragger</h1>
    <the-icon-list></the-icon-list>
  </div>
</template>

<script>
  import {defineComponent} from "vue";
  import TheIconList from "./TheIconList.vue";

  const App = defineComponent({
    name: "App",
    components: {
      TheIconList,
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
background: rgb(224,36,36);
background: linear-gradient(172deg, rgba(224,36,36,1) 12%, rgba(255,255,255,1) 53%);
}

#app { 
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
}
</style>