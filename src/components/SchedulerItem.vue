<script>
export default {
  props: {
    hour: Number,
    date: Object,
    taskName: String,
    taskImportance: Number,
    hasWork: Boolean,
    timeScale: Number,
  },
  data() {
    return {
      foundTask: {},
      hasTask: false,
    };
  },
  computed: {
  },
  mounted() {
  },
  methods: {
    taskImportanceColor() {
      switch (this.taskImportance) {
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
          return { background: "#DAF7A6" };
      }
    },
    selectedTime() {
      this.$emit("timeSelected", this.date, this.hour);
    },
  },
};
</script>

<template>
  <div v-if="!hasWork" :class="{ biggerItem: timeScale >= 1 }" class="item break"></div>
  <div v-else-if="taskName==null" class="item" :class="{ biggerItem: timeScale >= 1 }" @click="selectedTime"></div>
  <div v-else class="item" :class="{ biggerItem: timeScale >= 1 }" :style="taskImportanceColor()" @click="selectedTime">
    <h2>{{ taskName }}</h2>
  </div>
</template>

<style scoped>
.item {
  display: inline-block;
  width: 100%;
  height: 50px;
  border: solid #555b6e;
  border-width: 1px;
  cursor: pointer;
  text-align: center;
  background-color: #faf9f9;
  overflow: hidden;
}

.break {
  cursor: default;
  background-color: #d3d3d3;
}

.biggerItem {
  height: 150px;
}

.item h2 {
  font-size: 14px;
  margin-top: 2px;
  font-weight: 550;
}

@media only screen and (max-width: 800px) {
  .item h2 {
    font-size: 10px;
    margin-top: 5px;
  }
}

@media only screen and (max-width: 450px) {
  .item h2 {
    font-size: 8px;
    margin-top: 6px;
  }
}
</style>
