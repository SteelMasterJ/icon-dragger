<template>
  <div class='container mt-5'>
    <div class="row">
      <div 
      class="col-6 col-sm-3 col-lg-2" 
      v-for="(item, index) in IconList" 
      :class="item.class" :key="item.id" 
      :id="`drop-zone-${index}`" 
      @drop="drop($event, index)"
      @dragover.prevent
      @dragenter.prevent>
        <div 
        class="drop-zone bg-light p-3 mt-4" 
        :draggable="item.isDraggable"
        @mousedown="mouseDown(index)" 
        @mouseup="clearTimeDrag(index)" 
        @mouseleave="clearTimeDrag(index)"
        @dragstart="dragStart($event, item.id)">
          <div class="d-flex justify-content-center flex-wrap">
            <img src="./assets/logo.png" height="80" width="80" draggable="false"/>
            <div class="text-primary align-self-center">{{item.label}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import {defineComponent, onMounted, ref} from "vue";
  const App = defineComponent({
    name: "App",
    setup() {

      //logic for Data
      const initialIcons = [
        {
          id: 0,
          label: 'Hello World 0',
          isDraggable: false,
          class: '',
        },
        {
          id: 1,
          label: 'Hello World 1',
          isDraggable: false,
          class: '',          
        },
        {
          id: 2,
          label: 'Hello World 2',
          isDraggable: false,
          class: '',          
        },
        {
          id: 3,
          label: 'Hello World 3',
          isDraggable: false,
          class: '',          
        },
        {
          id: 4,
          label: 'Hello World 4',
          isDraggable: false,
          class: '',          
        },
        {
          id: 5,
          label: 'Hello World 5',
          isDraggable: false,
          class: '',          
        },
        {
          id: 6,
          label: 'Hello World 6',
          isDraggable: false,
          class: '',          
        },
      ];

      let IconList = ref([]);
      onMounted(() => {
        IconList.value = initialIcons;
      })


      // Logic for Draggability
      let mouseTimer;

      function setIsDraggable(index) {
        IconList.value[index].isDraggable = true;
        IconList.value[index].class = 'bounce';
      }

      function mouseDown(index) {
        mouseTimer = window.setTimeout(function() {setIsDraggable(index)},1000);
      }

      function clearTimeDrag(index) {
        if (mouseTimer) {
          IconList.value[index].isDraggable = false;
          IconList.value[index].class = '';
          window.clearTimeout(mouseTimer);
        }
      }

      function dragStart(e, id) {
        console.log(e, id);
        e.dataTransfer.dropEffect = 'move';
        e.dataTransfer.effectAllowed = 'move';
        e.dataTransfer.setData('itemID', id);
      }

      function drop(e, index) {
        const itemId = e.dataTransfer.getData('itemID');

        let draggedItem = IconList.value[itemId];
        draggedItem.class = '';
        draggedItem.isDraggable = false;

        // const currentIndex = IconList.value.map(x => x.id).indexOf(itemId);
        // console.log("current index: ", currentIndex, "drop-zone index: ", index);
        IconList.value.splice(itemId, 1);
        IconList.value.splice(index, 0, draggedItem);
        console.log(draggedItem);
      }

      return {
        initialIcons,
        mouseDown,
        clearTimeDrag,
        IconList,
        dragStart,
        drop,
      }
    },
  })
  export default App
</script>

<style scoped>
.bounce {
  animation: bounce 1.5s ease infinite;
}
@keyframes bounce {
  15% { transform: scale(.9); }
  40%, 60% { transform: rotate(-10deg) scale(.9); }
  50% { transform: rotate(10deg) scale(.9); }
  70% { transform: rotate(0deg) scale(.9); }
  100% { transform: scale(1); }
}
</style>