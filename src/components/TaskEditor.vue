<script>
export default {
  props: {
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
      selectedTimeEnd: 5,
      taskColor: '#ffffff',
      showColorPallete: false,
      colorPallete: [
        '#FFEBEE',
        '#FFCDD2',
        '#EF9A9A',
        '#E57373',
        '#EF5350',
        '#E53935',
        '#D32F2F',
        '#C62828',
        '#B71C1C',
        '#FF8A80',
        '#FF5252',
        '#FF1744',
        '#D50000',
        '#FCE4EC',
        '#F8BBD0',
        '#F48FB1',
        '#F06292',
        '#EC407A',
        '#E91E63',
        '#D81B60',
        '#C2185B',
        '#AD1457',
        '#880E4F',
        '#FF80AB',
        '#FF4081',
        '#F50057',
        '#C51162',
        '#F3E5F5',
        '#E1BEE7',
        '#CE93D8',
        '#BA68C8',
        '#AB47BC',
        '#9C27B0',
        '#8E24AA',
        '#7B1FA2',
        '#6A1B9A',
        '#4A148C',
        '#EA80FC',
        '#E040FB',
        '#D500F9',
        '#AA00FF',
        '#EDE7F6',
        '#D1C4E9',
        '#B39DDB',
        '#9575CD',
        '#7E57C2',
        '#673AB7',
        '#5E35B1',
        '#512DA8',
        '#4527A0',
        '#311B92',
        '#B388FF',
        '#7C4DFF',
        '#651FFF',
        '#6200EA',
        '#E8EAF6',
        '#C5CAE9',
        '#9FA8DA',
        '#7986CB',
        '#5C6BC0',
        '#3F51B5',
        '#3949AB',
        '#303F9F',
        '#283593',
        '#1A237E',
        '#8C9EFF',
        '#536DFE',
        '#3D5AFE',
        '#304FFE',
        '#E3F2FD',
        '#BBDEFB',
        '#90CAF9',
        '#64B5F6',
        '#42A5F5',
        '#2196F3',
        '#1E88E5',
        '#1976D2',
        '#1565C0',
        '#0D47A1',
        '#82B1FF',
        '#448AFF',
        '#2979FF',
        '#2962FF',
        '#E1F5FE',
        '#B3E5FC',
        '#81D4FA',
        '#4FC3F7',
        '#29B6F6',
        '#03A9F4',
        '#039BE5',
        '#0288D1',
        '#0277BD',
        '#01579B',
        '#80D8FF',
        '#40C4FF',
        '#00B0FF',
        '#0091EA',
        '#E0F7FA',
        '#B2EBF2',
        '#80DEEA',
        '#4DD0E1',
        '#26C6DA',
        '#00BCD4',
        '#00ACC1',
        '#0097A7',
        '#00838F',
        '#006064',
        '#84FFFF',
        '#18FFFF',
        '#00E5FF',
        '#00B8D4',
        '#E0F2F1',
        '#B2DFDB',
        '#80CBC4',
        '#4DB6AC',
        '#26A69A',
        '#009688',
        '#00897B',
        '#00796B',
        '#00695C',
        '#004D40',
        '#A7FFEB',
        '#64FFDA',
        '#1DE9B6',
        '#00BFA5',
        '#E8F5E9',
        '#C8E6C9',
        '#A5D6A7',
        '#81C784',
        '#66BB6A',
        '#4CAF50',
        '#43A047',
        '#388E3C',
        '#2E7D32',
        '#1B5E20',
        '#B9F6CA',
        '#69F0AE',
        '#00E676',
        '#00C853',
        '#F1F8E9',
        '#DCEDC8',
        '#C5E1A5',
        '#AED581',
        '#9CCC65',
        '#8BC34A',
        '#7CB342',
        '#689F38',
        '#558B2F',
        '#33691E',
        '#CCFF90',
        '#B2FF59',
        '#76FF03',
        '#64DD17',
        '#F9FBE7',
        '#F0F4C3',
        '#E6EE9C',
        '#DCE775',
        '#D4E157',
        '#CDDC39',
        '#C0CA33',
        '#AFB42B',
        '#9E9D24',
        '#827717',
        '#F4FF81',
        '#EEFF41',
        '#C6FF00',
        '#AEEA00',
        '#FFFDE7',
        '#FFF9C4',
        '#FFF59D',
        '#FFF176',
        '#FFEE58',
        '#FFEB3B',
        '#FDD835',
        '#FBC02D',
        '#F9A825',
        '#F57F17',
        '#FFFF8D',
        '#FFFF00',
        '#FFEA00',
        '#FFD600',
        '#FFF8E1',
        '#FFECB3',
        '#FFE082',
        '#FFD54F',
        '#FFCA28',
        '#FFC107',
        '#FFB300',
        '#FFA000',
        '#FF8F00',
        '#FF6F00',
        '#FFE57F',
        '#FFD740',
        '#FFC400',
        '#FFAB00',
        '#FFF3E0',
        '#FFE0B2',
        '#FFCC80',
        '#FFB74D',
        '#FFA726',
        '#FF9800',
        '#FB8C00',
        '#F57C00',
        '#EF6C00',
        '#E65100',
        '#FFD180',
        '#FFAB40',
        '#FF9100',
        '#FF6D00',
        '#FBE9E7',
        '#FFCCBC',
        '#FFAB91',
        '#FF8A65',
        '#FF7043',
        '#FF5722',
        '#F4511E',
        '#E64A19',
        '#D84315',
        '#BF360C',
        '#FF9E80',
        '#FF6E40',
        '#FF3D00',
        '#DD2C00',
        '#EFEBE9',
        '#D7CCC8',
        '#BCAAA4',
        '#A1887F',
        '#8D6E63',
        '#795548',
        '#6D4C41',
        '#5D4037',
        '#4E342E',
        '#3E2723',
        '#FAFAFA',
        '#F5F5F5',
        '#EEEEEE',
        '#E0E0E0',
        '#BDBDBD',
        '#9E9E9E',
        '#757575',
        '#616161',
        '#424242',
        '#212121',
        '#ECEFF1',
        '#CFD8DC',
        '#B0BEC5',
        '#90A4AE',
        '#78909C',
        '#607D8B',
        '#546E7A',
        '#455A64',
        '#37474F',
        '#263238',
        '#000000',
      ]
    }
  },
  created() {
    this.findTask();
    // Šitos dvi eilutės nustato pradines vertes, kad būtų patogiau naudotis puslapių
    this.selectedTimeStart = this.hour;
    this.selectedTimeEnd = this.hour + this.timeScale;
  },
  computed: {
    hasTasksEditor() {
      this.showTaskEdit = false;
      return this.foundTask != null;
    },
    getTaskInfo() {
      if (this.foundTask) {
        return {
          taskName: this.foundTask.taskName,
          taskDesc: this.foundTask.taskDesc,
          taskHourStart: this.foundTask.taskHourStart,
          taskHourEnd: this.foundTask.taskHourEnd - this.timeScale,
          taskStatus: this.foundTask.taskStatus,
          taskColor: this.foundTask.taskColor
        }
      }
      return {
        taskName: '',
        taskDesc: '',
        taskHourStart: this.hour,
        taskHourEnd: this.hour,
        taskStatus: '',
        taskColor: '#15d42b'
      }
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
      if (this.breakTime[0] <= timeShiftWithoutBreaks[timeShiftWithoutBreaks.length - 1]) {
        timeShiftWithoutBreaks.splice(timeShiftWithoutBreaks.indexOf(this.breakTime[0]), this.breakTime.length);
      }
      // Apkarpome iki mūsų užduoties pabaigos (jei ne editinam, tada įmamas pasirinkto langelio laikas)
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
      if (this.tName.length === 0) {
        return false
      }
      if (this.tDesc.length > 1024 || this.tName.length > 32) {
        return false
      }
      if (this.selectedTimeStart >= this.selectedTimeEnd) {
        return false
      }
      return true;
    },
    newTask() {
      if (this.requirementsCheck()) {
        this.$emit('NewTask', this.tName, this.tDesc, this.selectedTimeStart, this.selectedTimeEnd, this.taskColor);
      }
    },
    saveEdit() {
      if (this.requirementsCheck()) {
        this.$emit('taskEdit', this.foundTask.id, this.tName, this.tDesc, this.selectedTimeStart, this.selectedTimeEnd, this.taskColor);
      }
    },
    findTask() {
      // Suranda užduotį pagal pasirinkto langelio laiką.
      this.foundTask = this.tasks.find(task =>
        task.taskHourStart <= this.hour &&
        task.taskHourEnd - this.timeScale >= this.hour
      );
    },
    showEditScreen() {
      // Nukopijuojame pradinės užduoties vertes, kad būtų patogiau keisti ją.
      this.showTaskEdit = true;
      this.tName = this.getTaskInfo.taskName;
      this.tDesc = this.getTaskInfo.taskDesc;
      this.selectedTimeStart = this.getTaskInfo.taskHourStart;
      this.selectedTimeEnd = this.getTaskInfo.taskHourEnd + this.timeScale;
      this.taskColor = this.getTaskInfo.taskColor;
    }
  },
}
</script>
<template>
  <div class="taskList">
    <div class="taskDate">
      <h1>{{ date }}</h1>
    </div>
    <div class="formContainer" v-if="!hasTasksEditor || showTaskEdit">
      <!--- Užduoties kūrimas/keitimas. Start --->
      <!--- Užduoties pavadinimas --->
      <label for="tname">Task name:</label><br>
      <input id="tname" type="text" v-model="tName"><br>
      <p class="errorMsg" v-if="tName.length === 0">This field can't be empty</p>
      <p class="errorMsg" v-if="tName.length > 32">Name can't be longer than 32 characters.</p>
      <!--- Užduoties pradžios pasirinkimas --->
      <label for="taskStartsAt">Task starts:</label><br>
      <select id="taskStartsAt" v-model.number="selectedTimeStart">
        <template v-for="n in cutShiftTimeForSelect[0]">
          <option :value="n">{{
              formattedTime[n / timeScale]
          }}</option>
        </template>
      </select><br>
      <!--- Užduoties pabaigos pasirinkimas --->
      <label for="taskEndsAt">Task ends:</label><br>
      <select v-model.number="selectedTimeEnd">
        <template v-for="n in cutShiftTimeForSelect[1]">
          <option :value='n + timeScale'>{{
              formattedTime[n / timeScale + 1]
          }}</option>
        </template>
      </select><br>
      <p class="errorMsg" v-if="selectedTimeStart >= selectedTimeEnd"> Task can't end before it
        starts
      </p>
      <!--- Užduoties spalvos pasirinkimas --->
      <label for="colorPicker">Task color:</label><br>
      <button @click="showColorPallete = !showColorPallete" :style="{ background: taskColor }">Pick color</button><br>
      <!--- Spalvų paletė --->
      <div v-if="showColorPallete" class="colorPallete">
        <ul role="listbox">
          <li v-for="color in colorPallete" @click="taskColor = color" :style="{ background: color }" role="option">
          </li>
        </ul>
      </div>
      <!--- Užduoties aprašymas --->
      <label for="tdesc">Task description:</label><br>
      <textarea id="tdesc" type="text" v-model="tDesc"></textarea><br>
      <p class="errorMsg" v-if="tDesc.length > 1024">Description can't be longer than 1024
        characters.
      </p>
      <!--- Užduoties create/edit mygtukai --->
      <button v-if="!showTaskEdit" @click="newTask">Create new task</button>
      <button v-else @click="saveEdit">Save edit</button>
    </div>
    <!--- Užduoties kūrimas/keitimas. End --->
    <div v-else>
      <!--- Informacija apie užduotį. Start --->
      <div class="taskContainer">
        <!--- Užduoties pavadinimas --->
        <h2 style="font-size: 40px">{{ getTaskInfo.taskName }}</h2>
        <!--- Užduoties pradžios - pabaigos laikas --->
        <h1>{{ formattedTime[getTaskInfo.taskHourStart / timeScale] }} - {{ formattedTime[getTaskInfo.taskHourEnd /
            timeScale + 1]
        }}</h1>
        <!--- Užduoties būsena --->
        <h1>Status: {{ getTaskInfo.taskStatus }}</h1>
        <!--- Užduoties aprašymas --->
        <p>{{ getTaskInfo.taskDesc }}</p>
      </div>
      <!--- Informacija apie užduotį. End --->
      <template v-if="foundTask.taskStatus === 'ongoing'">
        <button type="button" @click="$emit('finishTheTask', foundTask.id)">Finish task</button><br><br>
        <button type="button" @click="showEditScreen()">Edit task</button><br><br>
      </template>
      <button type="button" @click="$emit('taskDeletion', foundTask.id)">Delete task</button>
    </div>
  </div>
</template>
<style scoped>
.taskList {
  width: 30%;
  text-align: center;
  background-color: #78a1bb;
  color: white;
  border: solid #555B6E;
  border-width: 0px 1px 1px 1px;
  overflow-x: hidden;
  overflow-y: auto;
}

.formContainer {
  height: 590px;
}

.errorMsg {
  color: #ffaa00;
  font-size: 16px;
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
  height: 600px;
  width: 80%;
  margin: auto;
  overflow-x: hidden;
  overflow-y: auto;
}

.taskContainer h2 {
  line-height: normal;
}

.taskContainer p {
  line-height: normal;
  text-align: justify;
}

.colorPallete {
  display: block;
  width: 80%;
  margin-left: 10%;
  background-color: white;
  border: 1px solid;
  border-color: #555B6E;
  padding: 5px 0 0 5px;
  border-radius: 0.25em;
}

.colorPallete ul {
  display: block;
  list-style-type: disc;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 5px;
  overflow: hidden;
  padding: 0;
  margin: 0;
}

.colorPallete li {
  list-style: none;
  width: 15px;
  height: 15px;
  float: left;
  margin-right: 5px;
  margin-bottom: 5px;
  position: relative;
  cursor: pointer;
}

textarea {
  resize: none;
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
  height: 400px;
}

input {
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
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