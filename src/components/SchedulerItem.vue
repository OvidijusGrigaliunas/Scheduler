<script>
export default {
  props: {
    dateObj: Object,
    taskInfo: Object,
    noWork: Boolean,
  },
  methods: {
    tileColor() {
      if (this.taskInfo.status != 'finished') {
        switch (this.taskInfo.importance) {
          case 1:
            return { background: "#43AA8B" };
          case 2:
            return { background: "#90BE6D" };
          case 3:
            return { background: "#F9C74F" };
          case 4:
            return { background: "#F3722C" };
          case 5:
            return { background: "#F94144" };
          default:
            return { background: "#faf9f9" };
        }
      }
      return { background: "#db88cc" };
    },
  },
};
</script>

<template>
  <div v-if="noWork" class="item break"></div>
  <div v-else class="item" :style="tileColor()" @click="$emit('timeSelected', this.dateObj.day, this.dateObj.hour)">
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
