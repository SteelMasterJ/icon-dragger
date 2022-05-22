<template>
    <div class="row">
        <div
        class="col-6 col-sm-3 col-lg-2"
        v-for="(item, index) in IconList">
            <div
            class="m-3 bg-light bg-light rounded"
            :draggable="item.isDraggable"
            @mousedown="mouseDown(index)"
            @mouseup="clearTimeDrag(index)"
            @mouseleave="clearTimeDrag(index)"
            @dragstart="dragStart($event, item.position)"
            :class="item.class" 
            :key="item.id"
            :id="`drop-zone-${index}`"
            @drop="drop($event, index)"
            @dragover.prevent
            @dragenter.prevent>
                <base-icon :label="item.label" :icon="item.icon"></base-icon>
            </div>
        </div>
    </div>
</template>

<script>
  import {defineComponent, onMounted, ref} from "vue";
  import initialPokemon from "../data/data.js";
  import BaseIcon from "./BaseIcon.vue";
  const TheIconList = defineComponent({
    name: "TheIconList",
    components: {
        BaseIcon,
    },
    setup() {

        //setup up data
        let IconList = ref([]);
        onMounted(() => {
            IconList.value = initialPokemon;
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

        return {
            IconList,
            mouseDown,
            clearTimeDrag,
            dragStart,
            drop,
            sortIcons,
        }
    },
  })
  export default TheIconList
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
