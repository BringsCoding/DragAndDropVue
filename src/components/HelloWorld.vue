<template>
  <div
    class="dropZone"
    @drop="onDrop($event, 1)"
    @dragenter.prevent
    @dragover.prevent
  >
    <div
      v-for="item in getList(1)"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event, item)"
    >
      {{ item.title }}
    </div>
  </div>
  <div
    class="dropZone"
    @drop="onDrop($event, 2)"
    @dragenter.prevent
    @dragover.prevent
  >
    <div
      v-for="item in getList(2)"
      :key="item.id"
      class="drag-el"
      draggable="true"
      @dragstart="startDrag($event, item)"
    >
      {{ item.title }}
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const items = ref([
      { id: 0, title: "Item A", list: 1 },
      { id: 1, title: "Item B", list: 1 },
      { id: 2, title: "Item C", list: 2 },
    ]);

    const getList = (list) => {
      return items.value.filter((item) => item.list == list);
    };

    const startDrag = (event, item) => {
      console.log(item);
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id);
    };

    const onDrop = (event, list) => {
      const itemID = event.dataTransfer.getData("itemID");
      const item = items.value.find((item) => item.id == itemID);
      item.list = list;
    };

    return {
      getList,
      startDrag,
      onDrop,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dropZone {
  width: 50%;
  margin: 3.125rem auto;
  background-color: bisque;
  padding: 0.625rem;
  min-height: 0.625rem;
}

.drag-el {
  background-color: aquamarine;
  color: black;
  padding: 0.3125rem;
  margin-bottom: 0.625rem;
}

.drag-el:nth-last-of-type(1) {
  margin-bottom: 0;
}
</style>
