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
    timeScale: Number,
  },
  data() {
    return {
      showTaskEdit: false,
      foundTask: [],
      tName: '',
      tDesc: '',
      selectedImportance: 3,
      selectedTimeStart: 5,
      selectedTimeEnd: 5

    }
  },
  created() {
    // Su mounted kartais kelios funkcijos buvo atkartojamos 2 kartus, nes per vėlai atsinaujindavo foundTask
    this.findTask();
    // Šitos dvi eilutės nustato pradines vertes, kad būtų patogiau naudotis puslapių
    this.selectedTimeStart = this.hour;
    this.selectedTimeEnd = this.hour + this.timeScale;
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
          return 'Very low';
        case 2:
          return 'Low';
        case 3:
          return 'Moderate';
        case 4:
          return 'High';
        case 5:
          return 'Very high';
        default:
          return 'Unimportant';
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
      // Mums reikalingos užduotys, kurios prasideda vėliau negu dabartinis laikas
      let filteredTasks = this.tasks.filter(task => task.taskHourStart > hourEndLimit);
      if (filteredTasks.length > 0) {
        let filteredTasksStartHours = [];
        let filLength = filteredTasks.length;
        for (let i = 0; i < filLength; i++) {
          filteredTasksStartHours[i] = filteredTasks[i].taskHourStart;
        }
        // Surandame arčiausiai esančią užduotį nuo mūsų užduoties pradžios
        let lowestValueIndex = filteredTasksStartHours.indexOf(Math.min(...filteredTasksStartHours));
        return filteredTasks[lowestValueIndex].taskHourStart - this.timeScale;
      }
      return this.shiftTime[this.shiftTime.length - 1];
    },
    getTaskHourStartLimit() {
      let taskStart = this.getTaskInfo.taskHourStart;
      // Mums reikalingos užduotys, kurios baigiasi anksčiau negu prasideda naujoji
      let filteredTasks = this.tasks.filter(task => task.taskHourEnd - this.timeScale < taskStart);
      if (filteredTasks.length > 0) {
        let filteredTasksEndHours = [];
        let filLength = filteredTasks.length;
        for (let i = 0; i < filLength; i++) {
          filteredTasksEndHours[i] = filteredTasks[i].taskHourEnd;
        }
        let lowestValueIndex = filteredTasksEndHours.indexOf(Math.max(...filteredTasksEndHours));
        return filteredTasks[lowestValueIndex].taskHourEnd;
      }
      return this.shiftTime[0];
    },
    // Padalina shiftTime į dvi dalis.
    // Pirmoji dalis yra skirta Task starts select vertes gauti,
    // o antroji Task Ends select.
    // Sumažina if salygų kiekį html dalyje
    cutShiftTimeForSelect() {
      let timeCut = [];
      let timeShiftWithoutBreaks = [...this.shiftTime];
      // Pašalina laikus, kurie priklauso breakTime
      timeShiftWithoutBreaks.splice(timeShiftWithoutBreaks.indexOf(this.breakTime[0]), this.breakTime.length)
      // Apkarpome iki mūsų užduoties pabaigos (jei ne editinam, tada pasirinktas laikas įmamas)
      timeCut[0] = [...timeShiftWithoutBreaks];
      timeCut[0].length = timeCut[0].indexOf(this.getTaskInfo.taskHourEnd) + 1;
      // Jei yra daugiau task juostoje prieš pasirinktą laiką,
      // mes vėl jį apkarpome nuo artimiausio task pabaigos
      if (this.getTaskHourStartLimit != this.shiftTime[0]) {
        timeCut[0] = timeCut[0].slice(timeCut[0].indexOf(this.getTaskHourStartLimit));
      }
      // Antroji dalis, praktiškai tas pats, tik apkarpome nuo priešingos pusės
      timeCut[1] = [...timeShiftWithoutBreaks];
      timeCut[1] = timeCut[1].slice(timeCut[1].indexOf(this.getTaskInfo.taskHourStart));
      if (this.getTaskHourEndLimit != this.shiftTime[this.shiftTime.length - 1]) {
        timeCut[1].splice(timeCut[1].indexOf(this.getTaskHourEndLimit) + 1, timeCut[1].length);
      }
      return timeCut;
    }
  },
  methods: {
    requirementsCheck() {
      let errorList = [];
      if (this.tName.length === 0 || this.tDesc.length === 0) {
        errorList.push('Please fill in all fields');
      }
      if (this.tDesc.length > 1024) {
        errorList.push('Description - maximum characters allowed: 1024');
      }
      if (this.tName.length > 64) {
        errorList.push('Name - maximum characters allowed: 64');
      }
      if (this.selectedTimeStart >= this.selectedTimeEnd) {
        errorList.push("Task can't end before it starts");
      }
      return errorList;
    },
    newTask() {
      let errorList = this.requirementsCheck();
      if (errorList.length > 0) {
        alert(errorList.join('. '));
      } else {
        this.$emit('NewTask', this.tName, this.tDesc, this.selectedTimeStart, this.selectedImportance, this.selectedTimeEnd);
      }
    },
    saveEdit() {
      let errorList = this.requirementsCheck();
      if (errorList.length > 0) {
        alert(errorList.join('. '));
      } else {
        this.$emit('taskEdit', this.foundTask, this.tName, this.tDesc, this.selectedTimeStart, this.selectedImportance, this.selectedTimeEnd);
      }
    },
    findTask() {
      // Suranda užduotį pagal pasirinkto langelio laiką,
      this.foundTask = this.tasks.find(task =>
        task.taskHourStart <= this.hour &&
        task.taskHourEnd - this.timeScale >= this.hour
      );
    },
    showEditScreen() {
      this.showTaskEdit = true;
      this.tName = this.getTaskInfo.taskName;
      this.tDesc = this.getTaskInfo.taskDesc;
      this.selectedTimeStart = this.getTaskInfo.taskHourStart;
      this.selectedTimeEnd = this.getTaskInfo.taskHourEnd + this.timeScale;
      this.selectedImportance = this.getTaskInfo.taskImportance;
    }
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
      <label for="tname">Task name:</label><br>
      <input type="text" v-model="tName"><br>
      <label for="taskStartsAt">Task starts:</label><br>
      <select v-model.number="selectedTimeStart">
        <template v-for="n in cutShiftTimeForSelect[0]">
          <option :value="n">{{
              formattedTime[n / timeScale]
          }}</option>
        </template>
      </select><br>
      <label for="taskEndsAt">Task ends:</label><br>
      <select v-model.number="selectedTimeEnd">
        <template v-for="n in cutShiftTimeForSelect[1]">
          <option :value='n + timeScale'>{{
              formattedTime[n / timeScale + 1]
          }}</option>
        </template>
      </select><br>
      <label for="importanceSelect">Importance level:</label><br>
      <select v-model.number="selectedImportance">
        <option :value='1'>Very low</option>
        <option :value='2'>Low</option>
        <option :value='3'>Moderate</option>
        <option :value='4'>High</option>
        <option :value='5'>Very high</option>
      </select><br>
      <label>Task description:</label><br>
      <textarea :style="setTextAreaHeight" type="text" v-model="tDesc"></textarea><br class="lineBreak"><br
        class="lineBreak">
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
      <button type="button" @click="showEditScreen()">Edit task</button><br class="lineBreak"><br class="lineBreak">
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