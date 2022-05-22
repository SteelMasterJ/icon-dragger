<template>
    <div class="row">
        <img class="poke-icon mx-auto my-3" :src="getImageUrl(icon)" draggable="false"/>
        <div class="text-center text-dark align-self-center mb-3 h5">{{label}}</div>
    </div>
</template>

<script>
  import {defineComponent} from "vue";

  const BaseIcon = defineComponent({
    name: "BaseIcon",
    props: {
        label: {type: String},
        icon: {type: String},
    },
    setup() {
        function getImageUrl(icon) {
            return new URL(`../assets/${icon}`, import.meta.url).href
        }

        return {
            getImageUrl,
        }
    },
  })
  export default BaseIcon
</script>

<style scoped>
.poke-icon {
  max-width: 80px;
}
</style>