<template>
  <div class='container mt-5'>
    <div class="row">
      <div
      class="col-6 col-sm-3 col-lg-2 bg-light m-3"
      v-for="(item, index) in IconList"
      :class="item.class" :key="item.id"
      :id="`drop-zone-${index}`"
      @drop="drop($event, index)"
      @dragover.prevent
      @dragenter.prevent>
        <div
        class="p-3"
        :draggable="item.isDraggable"
        @mousedown="mouseDown(index)"
        @mouseup="clearTimeDrag(index)"
        @mouseleave="clearTimeDrag(index)"
        @dragstart="dragStart($event, item.position)">
          <div class="row">
            <img class="ratio ratio-1x1" src="./assets/logo.png" draggable="false"/>
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
          label: 'Charizard',
          isDraggable: false,
          class: '',
          position: 0,
        },
        {
          id: 1,
          label: 'Blastoise',
          isDraggable: false,
          class: '',
          position: 1,
        },
        {
          id: 2,
          label: 'Venosaur',
          isDraggable: false,
          class: '',
          position: 2,
        },
        {
          id: 3,
          label: 'Pikachu',
          isDraggable: false,
          class: '',
          position: 3,
        },
        {
          id: 4,
          label: 'Lapras',
          isDraggable: false,
          class: '',
          position: 4,
        },
        {
          id: 5,
          label: 'Hitmonchan',
          isDraggable: false,
          class: '',
          position: 5,
        },
        {
          id: 6,
          label: 'Zapdos',
          isDraggable: false,
          class: '',
          position: 6,
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
        IconList.value[index].class = 'bounce-once';
      }
      function mouseDown(index) {
        mouseTimer = window.setTimeout(function() {setIsDraggable(index)},500);
      }
      function clearTimeDrag(index) {
        if (mouseTimer) {
          IconList.value[index].isDraggable = false;
          IconList.value[index].class = '';
          window.clearTimeout(mouseTimer);
        }
      }
      //dragging logic
      function dragStart(e, position) {
        e.dataTransfer.dropEffect = 'move';
        e.dataTransfer.effectAllowed = 'move';
        e.dataTransfer.setData('itemPosition', position);
        IconList.value[position].class = 'bounce';
      }
      function drop(e, index) {
        const itemPosition = e.dataTransfer.getData('itemPosition');
        IconList.value[itemPosition].class = '';
        let draggedItem = IconList.value[itemPosition];
        draggedItem.class = '';
        draggedItem.isDraggable = false;
        if(itemPosition > index) {
          draggedForward(index, itemPosition);
        } else if (itemPosition < index) {
          draggedBackward(index, itemPosition);
        }
        sortIcons();
        console.log(IconList.value);
      }
      function draggedBackward(dropIndex, draggedIndex) {
        for(let i=parseInt(draggedIndex); i <= dropIndex; i++) {
          // console.log(IconList.value[0].position);
          IconList.value[i].position = (IconList.value[i].position -1);
        }
        IconList.value[draggedIndex].position = dropIndex;
      }
      function draggedForward(dropIndex, draggedIndex) {
        for(let i=dropIndex; i < draggedIndex; i++) {
          IconList.value[i].position = IconList.value[i].position +1;
        }
        IconList.value[draggedIndex].position = dropIndex;
      }
      function sortIcons() {
        IconList.value.sort(byPosition);
      }
      function byPosition(a, b) {
        if (a.position < b.position) { return -1 }
        else if (a.position > b.position) { return 1 }
        return 0;
      }
      function dragOver(index) {
        console.log(index);
      }
      return {
        initialIcons,
        mouseDown,
        clearTimeDrag,
        IconList,
        dragStart,
        drop,
        sortIcons,
        dragOver,
      }
    },
  })
  export default App
</script>

<style scoped>
.bounce {
  animation: bounce 1.5s ease infinite;
}
.bounce-once {
  animation: bounce 1.5s ease 1;
}
@keyframes bounce {
  15% { transform: scale(.9); }
  40%, 60% { transform: rotate(-10deg) scale(.9); }
  50% { transform: rotate(10deg) scale(.9); }
  70% { transform: rotate(0deg) scale(.9); }
  100% { transform: scale(1); }
}
</style>