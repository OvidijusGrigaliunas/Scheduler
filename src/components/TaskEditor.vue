<script>
export default {
  props: {
    resolution: Number,
    hour: Number,
    date: String,
    tasks: Array,
    formattedTime: Array,
    shiftTime: Array,
    breakTime: Array,
    timeScale: Number
  },
  data() {
    return {
      showTaskEdit: false,
      foundTask: null
    }
  },
  mounted() {
    this.findTask();
  },
  computed: {
    hasTasksEditor() {
      if (this.foundTask != null) {
        this.showTaskEdit = false;
        return true;
      }
      this.showTaskEdit = false;
      return false;
    },
    getTaskImportanceText() {
      switch (this.foundTask.taskImportance) {
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
    getTaskInfo() {
      if (this.foundTask) {
        return {
          taskName: this.foundTask.taskName,
          taskDesc: this.foundTask.taskDesc,
          taskHourStart: this.foundTask.taskHourStart,
          taskHourEnd: this.foundTask.taskHourEnd - this.timeScale,
          taskImportance: this.foundTask.taskImportance
        }
      }
      return {
        taskName: '',
        taskDesc: '',
        taskHourStart: this.hour,
        taskHourEnd: this.hour,
        taskImportance: 3
      }
    },
    // Kad nebūtų kelių užduočių tuo pačiu metu, reikia, kad šita funkcija aptiktų galimą laiko pasirinkimo diapazoną

    setTextAreaHeight() {
      let textAreaHeight = this.resolution * 0.5;
      if (textAreaHeight <= 380) {
        // kuo yra maženis textAreaHeigth, tuo labiau turime jį mažinti
        // naudojame -1 pakelimo laipsnį, kad didesni skaičiai taptų mažesni už kitus 
        // pvz.: 2^-1 > 7^-1, 1/2 > 1/7
        let exponent = Math.pow((textAreaHeight / 70), -1) * 27;
        textAreaHeight = textAreaHeight * Math.pow(0.9, exponent);
      } else if (textAreaHeight > 500) {
        let exponent = Math.pow((textAreaHeight / 70), 2) / 20;
        textAreaHeight = textAreaHeight * Math.pow(1.7, exponent);
      }
      textAreaHeight = textAreaHeight + 'px';
      return { height: textAreaHeight };
    },
    setTaskTextHeight() {
      let textAreaHeight = this.resolution * 0.8;
      if (textAreaHeight <= 400) {
        let exponent = Math.pow((textAreaHeight / 70), -1) * 2;
        textAreaHeight = textAreaHeight * Math.pow(0.9, exponent);
      } else if (textAreaHeight > 500) {
        let exponent = Math.pow((textAreaHeight / 70), 1) / 100;
        textAreaHeight = textAreaHeight * Math.pow(1.7, exponent);
      }
      textAreaHeight = textAreaHeight + 'px';
      return { height: textAreaHeight };
    },
    getTaskHourEndLimit() {
      let hourEndLimit = this.getTaskInfo.taskHourStart;
      // Mums reikalingos užduotis, kurios prasideda vėliau negu dabartinis laikas
      let filteredTasks = this.tasks.filter(task => task.taskHourStart > hourEndLimit);
      if (filteredTasks.length > 0) {
        let filteredTasksStartHours = [];
        let filLength = filteredTasks.length;
        for (let i = 0; i < filLength; i++) {
          filteredTasksStartHours.push(filteredTasks[i].taskHourStart)
        }
        // Surandame arčiausiai esančią užduotį nuo mūsų užduoties pradžios
        let lowestValueIndex = filteredTasksStartHours.indexOf(Math.min(...filteredTasksStartHours));
        return filteredTasks[lowestValueIndex].taskHourStart - this.timeScale;
      }
      return this.shiftTime[this.shiftTime.length - 1];

    },
    getTaskHourStartLimit() {
      let taskStart = this.getTaskInfo.taskHourStart;
      // Mums reikalingos užduotis, kurios baigiasi anksčiau negu prasideda naujoji
      let filteredTasks = this.tasks.filter(task => task.taskHourEnd - this.timeScale < taskStart);
      if (filteredTasks.length > 0) {
        let filteredTasksEndHours = [];
        let filLength = filteredTasks.length;
        for (let i = 0; i < filLength; i++) {
          filteredTasksEndHours.push(filteredTasks[i].taskHourEnd);
        }
        let lowestValueIndex = filteredTasksEndHours.indexOf(Math.max(...filteredTasksEndHours));
        return filteredTasks[lowestValueIndex].taskHourEnd;
      }
      return this.shiftTime[0];
    },
  },
  methods: {
    requirementsCheck() {
      let errorList = [];
      if (tname.value.length === 0 || tdesc.value.length === 0) {
        errorList.push('Please fill in all fields');
      }
      if (tdesc.value.length > 1024) {
        errorList.push('Description - maximum characters allowed: 1024');
      }
      if (tname.value.length > 64) {
        errorList.push('Name - maximum characters allowed: 64');
      }
      if (parseFloat(taskStartsAt.value) >= parseFloat(taskEndsAt.value)) {
        errorList.push("Task can't end before it starts");
      }
      return errorList;
    },
    newTask() {
      let errorList = this.requirementsCheck();
      if (errorList.length > 0) {
        alert(errorList.join('. '))
      } else {
        this.$emit('NewTask', tname.value, tdesc.value, this.date, parseFloat(taskStartsAt.value), parseInt(importanceSelect.value), parseFloat(taskEndsAt.value));
      }
    },
    saveEdit() {
      let errorList = this.requirementsCheck();
      if (errorList.length > 0) {
        alert(errorList.join('. '))
      } else {
        this.$emit('taskEdit', this.foundTask, tname.value, tdesc.value, this.date, parseFloat(taskStartsAt.value), parseInt(importanceSelect.value), parseFloat(taskEndsAt.value));
      }
    },
    findTask() {
      // Suranda užduotį pagal pasirinktą langelį,
      // kuris gali būti užduoties diapazone
      this.foundTask = this.tasks.find(task =>
        task.taskHourStart <= this.hour &&
        task.taskHourEnd - this.timeScale >= this.hour
      );
    },

  },
}

</script>
<template>
  <div class="taskList">
    <div class="taskDate">
      <h1>{{ date }}</h1>
    </div>
    <div v-if="!hasTasksEditor || showTaskEdit">
      <!--- Užduoties kūrimas/keitimas  --->
      <label for="tname">Task name: </label><br>
      <input :value="getTaskInfo.taskName" type="text" id="tname" name="tname"><br>
      <label for="taskStartsAt">Task starts: </label><br>
      <select name="taskStartsAt" id="taskStartsAt">
        <template v-for="n in shiftTime">
          <template v-if="n >= getTaskHourStartLimit && n <= getTaskInfo.taskHourEnd">
            <option v-if="!breakTime.includes(n)" :selected="getTaskInfo.taskHourStart === n" :value='n'>{{
                formattedTime[parseInt(n / timeScale)]
            }}</option>
          </template>
        </template>
      </select><br>
      <label for="taskEndsAt">Task ends: </label><br>
      <select name="taskEndsAt" id="taskEndsAt">
        <template v-for="n in shiftTime">
          <template v-if="n >= getTaskInfo.taskHourStart && n <= getTaskHourEndLimit">
            <option v-if="!breakTime.includes(n)" :selected="getTaskInfo.taskHourEnd === n" :value='n + timeScale'>{{
                formattedTime[n / timeScale + 1]
            }}</option>
          </template>
        </template>
      </select><br>
      <label for="importanceSelect">Importance level: </label><br>
      <select name="importanceSelect" id="importanceSelect">
        <option :selected="getTaskInfo.taskImportance === 1" :value='1'>Very low</option>
        <option :selected="getTaskInfo.taskImportance === 2" :value='2'>Low</option>
        <option :selected="getTaskInfo.taskImportance === 3" :value='3'>Moderate</option>
        <option :selected="getTaskInfo.taskImportance === 4" :value='4'>High</option>
        <option :selected="getTaskInfo.taskImportance === 5" :value='5'>Very high</option>
      </select><br>
      <label for="tdesc">Task description: </label><br>
      <textarea :value="getTaskInfo.taskDesc" :style="setTextAreaHeight" type="text" id="tdesc"
        name="tdesc"></textarea><br class="lineBreak"><br class="lineBreak">
      <button v-if="!showTaskEdit" @click="newTask">Create new task</button>
      <button v-else @click="saveEdit">Save edit</button>
    </div>
    <div v-else>
      <!--- Informacija apie užduotį  --->
      <div :style="setTaskTextHeight" class="taskContainer">
        <h2 style="font-size: 40px">{{ getTaskInfo.taskName }}</h2>
        <h1>{{ formattedTime[getTaskInfo.taskHourStart / timeScale] }} - {{ formattedTime[getTaskInfo.taskHourEnd /
            timeScale + 1]
        }}</h1>
        <h1>Importance: {{ getTaskImportanceText }}</h1>
        <p>{{ getTaskInfo.taskDesc }}</p>
      </div>
      <button type="button" @click="showTaskEdit = true;">Edit task</button><br class="lineBreak"><br class="lineBreak">
      <button type="button" @click="$emit('taskDeletion', foundTask)">Delete task</button>
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

.taskContainer h2 {
  line-height: normal;
}

.taskContainer p {
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

  textarea {
    font-size: 0.8rem;
  }

  .lineBreak {
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

  textarea {
    font-size: 0.6rem;
  }
}
</style>