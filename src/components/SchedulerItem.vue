<script>
export default {
  props: {
    dateObj: Object,
    taskInfo: Object,
    noWork: Boolean,
  },
};
</script>

<template>
  <div v-if="noWork" class="item break"></div>
  <div v-else class="item" :style="{background: this.taskInfo.color}" @click="$emit('timeSelected', this.dateObj.day, this.dateObj.hour)">
    <h2>{{ taskInfo.name }}</h2>
  </div>
</template>

<style scoped>
.item {
  display: inline-block;
  width: 100%;
  height: 60px;
  border: solid #555b6e;
  border-width: 1px;
  cursor: pointer;
  text-align: center;
  overflow: hidden;
  color: black;
}

.break {
  cursor: default;
  background-color: #d3d3d3;
}

.item h2 {
  font-size: 14px;
  margin-top: 10px;
  font-weight: 550;
}

@media only screen and (max-width: 800px) {
  .item h2 {
    font-size: 10px;
    margin-top: 16px;
  }
}

@media only screen and (max-width: 450px) {
  .item h2 {
    font-size: 8px;
    margin-top: 20px;
  }
}
</style>
