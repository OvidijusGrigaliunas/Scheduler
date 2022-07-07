<script>
export default {
  props:['resolution', 'day','hour', 'date', 'tasks', 'name', 'formattedTime'],
  data() {
    return {
      showTaskEdit: false,
    }
  },
  computed: {
    hasTasks() { 
      if (this.findTask.length > 0) {
        this.showTaskEdit = false;
        return true;
      } else {
        this.showTaskEdit = false;
        return false;
      }
    },
    findTask() {
      // Suranda užduotį pagal pasirinktą langelį,
      // kuris gali būti užduoties diapazone
      return this.tasks.filter(task => 
        task.taskDay === this.date &&
        task.taskHourStart <= this.hour &&
        task.taskHourEnd >= this.hour &&
        task.taskPerson === this.name);
    },
    getTaskImportanceText() {
      switch(this.findTask[0].taskImportance) {
        case 1:
          return 'Very low'
        case 2:
          return 'Low'
        case 3:
          return 'Moderate'
        case 4:
          return 'High'
        case 5:
          return 'Very high'
        default:
          return 'Unimportant'
      }
    },
    getTaskHourStart() { 
      if(this.findTask[0]) {
        return this.findTask[0].taskHourStart;
      } else {
        return this.hour;
      }
    },
    getTaskHourEnd() { 
      if(this.findTask[0]) {
        return this.findTask[0].taskHourEnd;
      } else {
        return this.hour;
      }
    },
    getTaskImportance() { 
      if(this.findTask[0]) {
        return this.findTask[0].taskImportance;
      } else {
        return 3;
      }
    },
    getTaskName() { 
      if(this.findTask[0]) {
        return this.findTask[0].taskName;
      } else {
        return '';
      }
    },
    getTaskDesc() { 
      if(this.findTask[0]) {
        return this.findTask[0].taskDesc;
      } else {
        return '';
      }
    },
    // Kad nebūtų kelių užduočių tuo metų, reikia, kad šita funkcija aptiktų galimą laiko pasirinkimo diapazoną
    getTaskHourEndLimit() {
      let hourEndLimit = this.getTaskHourStart;
      // Mums reikalingos užduotis, kurios prasideda vėliau negu dabartinis laikas
      let filteredTasks = this.tasks.filter(task => task.taskDay === this.date && task.taskPerson === this.name && task.taskHourStart > hourEndLimit);
      
      if(filteredTasks.length > 0) {
        let filteredTasksStartHours = [];
        for(let i = 0; i < filteredTasks.length; i++) {
          filteredTasksStartHours.push(filteredTasks[i].taskHourStart)
        }
        // Surandame arčiausiai esančią užduotį nuo mūsų užduoties pradžios
        let lowestValueIndex = filteredTasksStartHours.indexOf(Math.min(...filteredTasksStartHours));
        return filteredTasks[lowestValueIndex].taskHourStart - 0.5;
      } else {
        // Jei nėra daugiau užduočių po pasirinktos valandos, tai galime tiesiog gražinti 23.5
        return 23.5;
      }
      
    },
    getTaskHourStartLimit(){
      let taskStart = this.getTaskHourStart;
      // Mums reikalingos užduotis, kurios baigiasi anksčiau negu prasideda naujoji
      let filteredTasks = this.tasks.filter(task => task.taskDay === this.date && task.taskPerson === this.name && task.taskHourEnd < taskStart);
      if(filteredTasks.length > 0) {
        let filteredTasksEndHours = [];
        for(let i = 0; i < filteredTasks.length; i++) {
          filteredTasksEndHours.push(filteredTasks[i].taskHourEnd);
        }
        let lowestValueIndex = filteredTasksEndHours.indexOf(Math.max(...filteredTasksEndHours));
        return filteredTasks[lowestValueIndex].taskHourEnd + 0.5;
      } else {
        return 0;
      }
    },
    setTextAreaHeight() { 
      let textAreaHeight = this.resolution * 0.5;
      if(textAreaHeight <= 380) {
        // kuo yra maženis textAreaHeigth, tuo labiau turime jį mažinti
        // naudojame -1 pakelimo laipsnį, kad didesni skaičiai taptų mažesni už kitus 
        // pvz.: 2^-1 > 7^-1, 1/2 > 1/7
        let exponent = Math.pow((textAreaHeight / 70), -1) * 27;
        textAreaHeight = textAreaHeight * Math.pow(0.9, exponent);
      } else if(textAreaHeight > 500) {
         let exponent = Math.pow((textAreaHeight / 70), 2) / 20;
         textAreaHeight = textAreaHeight * Math.pow(1.7, exponent);
      }
      textAreaHeight = textAreaHeight + 'px';
      return {height: textAreaHeight};
    },
    setTaskTextHeight() {
      let textAreaHeight = this.resolution * 0.8;
      if(textAreaHeight <= 400) {
        let exponent = Math.pow((textAreaHeight / 70), -1) * 2;
        textAreaHeight = textAreaHeight * Math.pow(0.9, exponent);
      } else if(textAreaHeight > 500) {
        let exponent = Math.pow((textAreaHeight / 70), 1) / 100;
        textAreaHeight = textAreaHeight * Math.pow(1.7, exponent);
      }
      textAreaHeight = textAreaHeight + 'px';
      return {height: textAreaHeight};
    }
  },
  methods: {
    requirementsCheck(){
      let errorList = '';
      if (tname.value.length === 0 || tdesc.value.length === 0) {
        errorList = errorList + 'Please fill in all fields \r\n'
      } 
      if(tdesc.value.length > 1024 ) {
        errorList = errorList + 'Description - maximum characters allowed: 1024. \r\n'
      } 
      if(tname.value.length > 64 ) {
        errorList = errorList + 'Name - maximum characters allowed: 64. \r\n'
      } 
      if(parseFloat(taskStartsAt.value) > parseFloat(taskEndsAt.value)) {
        errorList = errorList + "Task can't end before it starts."
      } 
      return errorList;
    },
    newTask() {
      let errorList = this.requirementsCheck();     
      if(errorList.length > 0) { 
        alert(errorList)      
      } else {
        this.$emit('NewTask', tname.value, tdesc.value, this.date, parseFloat(taskStartsAt.value), parseInt(importanceSelect.value), parseFloat(taskEndsAt.value));
      } 
    },   
    saveEdit(){
      console.log(this.date)
      let errorList = this.requirementsCheck();
      if(errorList.length > 0) {
        alert(errorList)      
      } else {
        this.$emit('taskDeletion', this.findTask);
        this.newTask();
      } 
    }
  },
}

</script>
<template>
  <div class="taskList">
    <div class="taskDate">
      <h1>{{ date }}</h1>
    </div>
    <div v-if="!hasTasks || showTaskEdit">
      <!--- Užduoties kurimas/keitimas  --->
      <label for = "fname">Task name: </label><br>
      <input :value = "getTaskName" type = "text" id = "tname" name = "tname" ><br>
      <label for = "taskStartsAt">Task starts: </label><br>
      <select name = "taskStartsAt" id = "taskStartsAt">
        <template v-for = "n in 48">
          <template v-if = "(n - 1) / 2 >= getTaskHourStartLimit && (n - 1) / 2 <= getTaskHourEnd">
            <option :selected="getTaskHourStart === (n - 1) / 2" :value = '(n - 1) / 2' >{{ formattedTime[n - 1] }}</option>
          </template>
        </template>
      </select><br>
      <label for = "taskEndsAt">Task ends: </label><br>
      <select name = "taskEndsAt" id = "taskEndsAt">
        <template v-for = "n in 48">
          <template v-if = "(n - 1) / 2 >= getTaskHourStart && (n - 1) / 2 <= getTaskHourEndLimit">
            <option :selected="getTaskHourEnd === (n - 1) / 2" :value = '(n - 1) / 2' >{{ formattedTime[n - 1] }}</option>
          </template>
        </template>
      </select><br>
      <label for = "importanceSelect">Importance level: </label><br>
      <select  name = "importanceSelect" id = "importanceSelect">
        <option :selected="getTaskImportance === 1" :value = '1'>Very low</option>
        <option :selected="getTaskImportance === 2" :value = '2'>Low</option>
        <option :selected="getTaskImportance === 3" :value = '3'>Moderate</option>
        <option :selected="getTaskImportance === 4" :value = '4'>High</option>
        <option :selected="getTaskImportance === 5" :value = '5'>Very high</option>
      </select><br>
      <label for = "tdesc">Task description: </label><br>
      <textarea :value = "getTaskDesc" :style = "setTextAreaHeight" type = "text" id = "tdesc" name = "tdesc"></textarea><br class = "lineBreak"><br class = "lineBreak">
      <button v-if = "!showTaskEdit" @click = "newTask">Create new task</button>
      <button v-else @click = "saveEdit">Save edit</button>
    </div>
    <div v-else>
      <!--- Informacija apie užduotį  --->
        <div :style ="setTaskTextHeight" class="taskContainer">
          <h2 style = "font-size: 40px">{{ getTaskName }}</h2>
          <h1>{{ formattedTime[getTaskHourStart * 2] }} - {{ formattedTime[getTaskHourEnd * 2] }}</h1>
          <h1>Importance: {{ getTaskImportanceText }}</h1>
          <p>{{ getTaskDesc }}</p>
        </div>
      <button type = "button" @click = "showTaskEdit = true;">Edit task</button><br class = "lineBreak"><br class = "lineBreak">
      <button type = "button" @click = "$emit('taskDeletion', findTask)">Delete task</button>
    </div>
    
  </div>
</template>
<style scoped>
.taskList {
  width: 30%;
  height: fill;
  text-align: center;
  background-color: #78a1bb;
  color: white;
  border: solid #555B6E;
  border-width: 0px 1px 1px 1px;
}
.taskDate {
  height: 50px;
  width: 100%;
  border: solid #555B6E;
  border-width: 0px 0px 1px 0px;
  text-align: center;
  overflow: hidden;
}
.taskDate h1 {
  margin: 9px;
  line-height: normal;
}
h1 {
  font-size: 25px;
}
.taskContainer {
  height: 590px;
  width: 80%;
  margin: auto;
  overflow-x: hidden;
  
}
.taskContainer h2{
  line-height: normal;
}
.taskContainer p{
  line-height: normal;
  text-align: justify;
}
textarea {
  resize: none;
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
}
input {
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
}
select {
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
  cursor: pointer;
  line-height: 1.1;
  background-color: #fff;
  background-image: linear-gradient(to top, #f9f9f9, #fff 33%);
}
button {
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
  cursor: pointer;
  line-height: 1.1;
  background-color: #fff;
}
button:hover {
  background-image: linear-gradient(to top, #ffaa00, #fff 70%);
}
@media only screen and (max-width: 630px) {
  .taskContainer h1 {
    font-size: 22px;
  }
  .taskContainer h2 {
    font-size: 18px;
  }
  .taskContainer p {
    font-size: 14px;
  }
  .taskDate h1 {
  font-size: 22px;
  }
}
@media only screen and (max-width: 450px) {
  .taskContainer h1 {
    font-size: 15px;
  }
  .taskContainer h2 {
    font-size: 12px;
  }
  .taskContainer p { 
    font-size: 10px;
  }
  .taskDate h1 {
  font-size: 17px;
  margin-top: 13px;
  }
}
@media only screen and (max-height: 500px) {
  input {
    font-size: 0.8rem;
  }
  select {
    font-size: 0.8rem;
  }
  button {
    font-size: 0.8rem;
  }
  textarea{
    font-size: 0.8rem;
  }
  .lineBreak{
    display: none;
  }
}
@media only screen and (max-height: 400px) {
  input {
    font-size: 0.6rem;
  }
  select {
    font-size: 0.6rem;
  }
  button {
    font-size: 0.6rem;
  }
  textarea{
    font-size: 0.6rem;
  }
  
}
</style>