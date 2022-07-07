<script>
export default {
  props:['day', 'hour', 'date', 'tasks', 'name'],
  data() { 
    return {
    }
  },
  computed: {
        dateFormat() {
          let result = this.date.getFullYear() + "/"
          let month = this.date.getMonth() + 1;
          // Su if nustatome ar reikia pridėti 0 pradžioje,
          // kad gautume yyyy/mm/dd datos formatą
          if (month < 10) {
            result = result + 0 + month + "/";
          } else {
            result = result + month + "/";
          }
          if (this.date.getDate() < 10) {
            result = result + 0 + this.date.getDate()
          } else {
            result = result + this.date.getDate();
          }
          return result;
      },
        dayHasTasks() {
          if (this.findTask.length > 0) {
            return true;
          } else {
            return false;
          }
        },
        findTask() {
          return this.tasks.filter(task => 
          task.taskDay === this.dateFormat && 
          task.taskHourStart <= this.hour &&
          task.taskHourEnd >= this.hour &&
          task.taskPerson === this.name);
        },
        taskImportanceColor() {
          switch(this.findTask[0].taskImportance) {
            case 1:
              return {background: '#43AA8B'};
            case 2:
              return {background: '#90BE6D'};
            case 3:
              return {background: '#F9C74F'};
            case 4:
              return {background: '#F3722C'};
            case 5:
              return {background: '#F94144'};
            default:
              return {background: '#DAF7A6'}
          }
        }
    },
    methods: {
      selectedTime() {
        this.$emit('timeSelected', this.day, this.date, this.hour);
      }
    }
}
</script>

<template>
  <div v-if = "!dayHasTasks" class = "item" @click = "selectedTime" ></div>
  <div v-else class = "item" :style = "taskImportanceColor" @click = "selectedTime">
    <h2>{{ findTask[0].taskName }}</h2>
  </div>
</template>

<style scoped>
.item {
  display: inline-block;
  width: 100%;
  height: 30px;
  border: solid #555B6E;
  border-width: 1px;
  cursor: pointer;
  text-align: center;
  background-color: #FAF9F9;
  overflow: hidden;
  white-space: nowrap;
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