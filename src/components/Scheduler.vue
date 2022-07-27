<script setup>
import SchedulerItem from './SchedulerItem.vue'
import TaskEditor from './TaskEditor.vue'
import WeekSelector from './WeekSelector.vue';
import PersonSelector from './PersonSelector.vue';
</script>

<script>
// Čia yra parent, visi kiti jo child
export default {
  data() {
    return {
      taskArray: [
        // Jei rašo nuo 10:00 iki 15:00, tai reiškia , kad nuo 15:00 jau laisvas
        //{ taskName: 'task1', taskDesc: 'task1 desc', taskDay: "2022/07/26", taskHourStart: 8, taskHourEnd: 9, taskTarget: 'Vardenis', taskImportance: 5, taskStatus: 'ongoing' },
        { taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/07/26", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskImportance: 4, taskStatus: 'finished' },
        { taskName: 'task3', taskDesc: 'task3 desc', taskDay: "2022/07/26", taskHourStart: 10, taskHourEnd: 10.5, taskTarget: 'Vardenis', taskImportance: 3, taskStatus: 'ongoing' },
        { taskName: 'task4', taskDesc: 'task4 desc', taskDay: "2022/07/26", taskHourStart: 11, taskHourEnd: 11.5, taskTarget: 'Vardenis', taskImportance: 2, taskStatus: 'ongoing' },
      ],
      people: [
        { name: "Vardenis", shiftStart: 8, shiftEnd: 17, hasBreak: true, breakStart: 12, breakEnd: 13, workDays: [true, true, true, true, true, false, false] },
        { name: "Pavardenis", shiftStart: 9, shiftEnd: 19, hasBreak: true, breakStart: 12, breakEnd: 13, workDays: [true, true, true, true, true, false, false] },
        { name: "Bonilla", shiftStart: 10, shiftEnd: 19, hasBreak: true, breakStart: 11, breakEnd: 13, workDays: [true, true, true, true, true, false, true] },
        { name: "Hobbs", shiftStart: 7, shiftEnd: 16, hasBreak: false, breakStart: null, breakEnd: null, workDays: [true, false, true, true, true, false, false] }
      ],
      selectedHour: new Date().getHours(),
      selectedDate: new Date(),
      selectedDay: new Date().getDay(),
      linePosition: 0,
      showLine: true,
      selectedPersonIndex: 0,
      windowHeight: window.screen.availHeight - 197,
      filteredTasksByUsers: [],
      filteredTasks: [],
      currentWeek: [],
      taskEditorKey: 30,
      weekDayStrArr: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      // Veikia su 0.25, 0.5 ir 1
      // Jei 0.25, laiko tarpai yra 15min
      // 0.5 - 30 min; 1 - 1h
      // su vertėmis didesnėmis negu 1 neveikia
      // Pagal viską turėtų veikti su 0.1 ir 0.2 (6min ir 12min), bet vertės gaunamos su daug skaičių po kablelio.
      // Turi veikti su 1/2^x vertėmis, tik reikėtų pataisyti laiko formativimą truputi
      // Jei timeScale yra 0 susidaro endless loop
      timeScale: 0.25
    }
  },
  computed: {
    getShiftTimeArray() {
      let result = [];
      this.people.forEach((person, index) => {
        result[index] = [];
        for (let i = person.shiftStart; i < person.shiftEnd; i = i + this.timeScale) {
          result[index].push(i);
        }
      });
      return result;
    },
    getBreakTimeArray() {
      let result = [];
      this.people.forEach((person, index) => {
        result[index] = [];
        if (person.hasBreak) {
          for (let i = person.breakStart; i < person.breakEnd; i = i + this.timeScale) {
            result[index].push(i);
          }
        }
      });
      return result;
    },
    getWorkDaysNumb() {
      let result = new Array();
      for (let i = 0; i < 7; i++) {
        if (this.people[this.selectedPersonIndex].workDays[i] === true) {
          result.push(i);
        }
      }
      return result;
    },
    getWeekGridStyle() {
      return { gridTemplateColumns: '9.1% repeat(' + this.getWorkDaysNumb.length + ',' + 91 / this.getWorkDaysNumb.length + '%)' }
    },
    getGridStyle() {
      if (this.getWorkDaysNumb.length > 0) {
        return {
          height: this.getResolutionHeight + 'px',
          gridTemplateColumns: '9.1% repeat(' + this.getWorkDaysNumb.length + ',' + 91 / this.getWorkDaysNumb.length + '%)'
        }
      }
      return {
        height: this.getResolutionHeight + 'px',
        gridTemplateColumns: '9.1%'
      }
    },
    selectedDateFormatting() {
      let result = `${this.selectedDate.getFullYear()}/`;
      let month = this.selectedDate.getMonth() + 1;
      let day = this.selectedDate.getDate();
      // Nustatome ar reikia pridėti 0 pradžioje,
      // kad gautume yyyy/mm/dd datos formatą
      result = month < 10 ? `${result}0${month}/` : `${result}${month}/`;
      result = day < 10 ? `${result}0${day}` : `${result}${day}`;
      return result;
    },
    getResolutionHeight() {
      let height = this.windowHeight - 16;
      if (height < 750 && height > 1000) {
        height = height + 80;
      }
      return height;
    },
    formatTime() {
      let timeArray = [];
      // 60 * (i % 1) atitinka minutes. Iš i % 1 galime gauti 0, 0.25, 0.5, 0.75 (priklauso nuo timeScale). Šitas vertes padauginus iš 60 gauname minučių skaičių.
      // Jei tas skaičius yra mažesnis už 10, prie minučių reikia pridėti 0, kad būtų tvarkingai pateiktas laikas. 
      for (let i = 0; i < 10; i = i + this.timeScale) {
        let minutes = 60 * (i % 1);
        let hours = i - (i % 1);
        timeArray.push(minutes > 10 ? `0${hours}:${minutes}` : `0${hours}:0${minutes}`);
      }
      for (let i = 10; i < 24; i = i + this.timeScale) {
        let minutes = 60 * (i % 1);
        let hours = i - (i % 1);
        timeArray.push(minutes > 10 ? `${hours}:${minutes}` : `${hours}:0${minutes}`);
      }
      timeArray.push('00:00')
      return timeArray;
    },
    // Sugeneruoja 2d array, pagal kurią galime žinoti ar langelis turi užduotį. Šios funckijos pagalba persiunčiame langeliams reikalinga info
    generatedTaskInfoForItems() {
      let taskInfoArray = new Array(7).fill().map(() => new Array(this.getShiftTimeArray[this.selectedPersonIndex].length).fill({ name: null, importance: null, status: null }));
      for (let i = 0; i < 7; i++) {
        this.filteredTasks[i].forEach(task => {
          taskInfoArray[i] = this.updateColumnTaskInfo(taskInfoArray[i], task);
        })
      }
      return taskInfoArray;
    },
  },
  mounted() {
    window.onresize = () => {
      this.windowHeight = window.screen.availHeight - 197;
    }
    setInterval(this.showCurTime, 30000);
  },
  created() {
    this.taskInfoArray = new Array(7).fill().map(() => new Array(this.getShiftTimeArray[0].length).fill({ name: null, importance: null }));
    this.createWeek(this.selectedDate);
    this.filterTaskByUsers();
    this.showCurTime();
  },
  watch: {
    people: {
      handler() {
        this.showCurTime()
      },
      deep: true
    }
  },
  methods: {
    showCurTime() {
      let lineCurrentTime = new Date();
      let end = this.getShiftTimeArray[this.selectedPersonIndex][this.getShiftTimeArray[this.selectedPersonIndex].length - 1];
      let start = this.getShiftTimeArray[this.selectedPersonIndex][0];
      let position = ((lineCurrentTime.getHours() - start) * 60 + lineCurrentTime.getMinutes()) / this.timeScale;
      if (lineCurrentTime.getHours() + lineCurrentTime.getMinutes() / 60 < end + this.timeScale && lineCurrentTime.getHours() + lineCurrentTime.getMinutes() / 60 > start) {
        this.showLine = true;
        this.linePosition = {
          marginTop: position + "px"
        };
      } else {
        this.showLine = false;
      }
    },
    // Keičiant vartotoją dažnai pasirinkta valanda išeidavo iš darbo laiko ribų
    // Ši funkcija tai pataiso
    getSelectedHour() {
      if (
        !this.getShiftTimeArray[this.selectedPersonIndex].includes(this.selectedHour) ||
        this.getBreakTimeArray[this.selectedPersonIndex].includes(this.selectedHour)
      ) {
        this.selectedHour = this.people[this.selectedPersonIndex].shiftStart;
        this.taskEditorKey = this.taskEditorKey * (-1);
      }
    },
    createNewTask(name, desc, startsAt, importance, endsAt) {
      let object = {
        taskName: name,
        taskDesc: desc,
        taskDay: this.selectedDateFormatting,
        taskHourStart: startsAt,
        taskHourEnd: endsAt,
        taskTarget: this.people[this.selectedPersonIndex].name,
        taskImportance: importance,
        taskStatus: 'ongoing'
      }
      this.taskArray.push(object);
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    deleteTask(taskToDelete) {
      let arrLength = this.taskArray.length;
      for (let i = 0; i < arrLength; i++) {
        if (taskToDelete === this.taskArray[i]) {
          this.taskArray.splice(i, 1);
          break;
        }
      }
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    editTask(taskToChange, name, desc, startsAt, importance, endsAt) {
      this.taskArray[this.taskArray.indexOf(taskToChange)] = {
        taskName: name,
        taskDesc: desc,
        taskDay: this.selectedDateFormatting,
        taskHourStart: startsAt,
        taskHourEnd: endsAt,
        taskTarget: this.people[this.selectedPersonIndex].name,
        taskImportance: importance,
        taskStatus: 'ongoing'
      };
      this.selectedHour = startsAt;
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    finishTask(taskToFinish) {
      this.taskArray[this.taskArray.indexOf(taskToFinish)].taskStatus = 'finished';
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    timeSelection(dateIndex, hour) {
      this.selectedDay = dateIndex + 1;
      this.selectedHour = hour;
      this.selectedDate = this.currentWeek[dateIndex];
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    addDays(date, days) {
      let newDate = new Date(date);
      newDate.setDate(newDate.getDate() + days);
      return newDate;
    },
    // Sudarome savaitę pagal pasirinktą diena
    createWeek(date) {
      // kai yra sekmadienis, vertė būna 0
      // tokiu atveju 0 reikia pasiversti į 7
      let currentWeekDay = date.getDay() != 0 ? date.getDay() : 7;
      for (let i = 0; i < 7; i++) {
        // "i + 1 - currentWeekDay" reikalingas, kad savaitės dienos eitų iš eilės
        // (Monday, tuesday ir t.t.)
        this.currentWeek[i] = this.addDays(date, i + 1 - currentWeekDay);
      }
      this.getSelectedHour();
    },
    changeWeek(direction) {
      this.selectedDate = this.addDays(this.selectedDate, direction);
      this.createWeek(this.selectedDate);
      this.filterTasksByWeek(this.currentWeek);
      this.taskEditorKey = this.taskEditorKey * (-1);
    },
    changePerson(direction) {
      let newIndex = this.selectedPersonIndex + direction;
      // Jei išeiname iš masyvo ribų, pagal direction nustatome ar turime peršokti į masyvo
      // pradžią ar pabaigą. Jei direction -1 - pabaiga, jei 1 - pradžia;
      this.selectedPersonIndex = this.people[newIndex] ? newIndex : this.people.length - newIndex * direction;
      this.getSelectedHour();
      this.filterTaskByUsers();
      this.taskEditorKey = this.taskEditorKey * (-1);
      this.showCurTime();
    },
    filterTasksByWeek() {
      let formattedWeek = this.dateFormatting(this.currentWeek);
      // Naudojame 2d array, kad galėtume lengvai paskirstyti užduotis į
      // savaitės dienas.
      for (let i = 0; i < 7; i++) {
        this.filteredTasks[i] = [];
      }
      this.filteredTasksByUsers.forEach(task => {
        for (let i = 0; i < 7; i++) {
          if (task.taskDay === formattedWeek[i]) {
            this.filteredTasks[i].push(task);
            break;
          }
        }
      });
    },
    filterTaskByUsers() {
      this.filteredTasksByUsers = this.taskArray.filter(task => task.taskTarget === this.people[this.selectedPersonIndex].name);
      this.filterTasksByWeek();
    },
    dateFormatting(week) {
      let result = [];
      week.forEach(date => {
        let month = date.getMonth() + 1;
        let day = date.getDate();
        let formattedDay = month < 10 ? `${date.getFullYear()}/0${month}/` : `${date.getFullYear()}/${month}/`;
        formattedDay = day < 10 ? `${formattedDay}0${day}` : `${formattedDay}${day}`;
        result.push(formattedDay);
      });
      return result;
    },
    updateColumnTaskInfo(taskArray, task) {
      let taskHourEndIndex = (task.taskHourEnd - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
      let taskHourStartIndex = (task.taskHourStart - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
      for (let i = taskHourStartIndex; i < taskHourEndIndex; i++) {
        taskArray[i] = { name: task.taskName, importance: task.taskImportance, status: task.taskStatus }
      }
      return taskArray;
    },
    changeDate(year, month, day) {
      this.selectedDate = new Date(year, month - 1, day);
      this.selectedDay = this.selectedDate.getDay() != 0 ? this.selectedDate.getDay() : 7;
      this.createWeek(this.selectedDate);
      this.filterTasksByWeek();
      this.taskEditorKey = this.taskEditorKey * (-1);
    }
  }
}
</script>

<template>
  <PersonSelector :peopleList='people' :selectedIndex='selectedPersonIndex' :timeScale='timeScale'
    :formattedTime='formatTime' @personChange="changePerson" />
  <div class="container">
    <div class="gridContainer">
      <div class="gridAndDateContainer">
        <div class="selectContainer">
          <WeekSelector :weekRange='currentWeek' :person="people[selectedPersonIndex]" @weekChange="changeWeek"
            @changeDate="changeDate" />
        </div>
        <div class="grid" :style="getWeekGridStyle">
          <div class="time blankRectangle"></div>
          <template v-for="(bool, index) in people[selectedPersonIndex].workDays">
            <div v-if="bool === true" class="weekDay">
              <p>{{ weekDayStrArr[index] }}</p>
            </div>
          </template>
        </div>
        <div class="grid" :style="getGridStyle">
          <div v-show="showLine" class="timeLine" :style="linePosition"></div>
          <template v-for='(i, hourIndex) in getShiftTimeArray[selectedPersonIndex]'>
            <div class="time">
              <h1>{{ formatTime[i / timeScale] }}</h1>
            </div>
            <template v-for="dayIndex in getWorkDaysNumb">
              <SchedulerItem v-if="getBreakTimeArray[selectedPersonIndex].includes(i)" :noWork='true' />
              <SchedulerItem v-else :dateObj='{ day: dayIndex, hour: i }'
                :taskInfo='generatedTaskInfoForItems[dayIndex][hourIndex]' @timeSelected="timeSelection" />
            </template>
          </template>
        </div>
      </div>
      <TaskEditor :key="taskEditorKey" :resolution='getResolutionHeight' :date='selectedDateFormatting'
        :tasks='filteredTasks[selectedDay - 1]' :hour='selectedHour' :formattedTime='formatTime'
        :breakTime='getBreakTimeArray[selectedPersonIndex]' :shiftTime='getShiftTimeArray[selectedPersonIndex]'
        :timeScale='timeScale' @NewTask="createNewTask" @finishTheTask="finishTask" @taskDeletion="deleteTask"
        @taskEdit='editTask' />
    </div>
  </div>
</template>

<style scoped>
.gridContainer {
  display: flex;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.gridAndDateContainer {
  width: 70%;
  -ms-overflow-style: none;
  scrollbar-width: none;
  overflow: auto;
}

.container {
  margin: auto;
  overflow-x: hidden;
  width: 100%;
  border: solid #555B6E;
  border-width: 1px 1px 1px 1px;
}

.selectContainer {
  display: flex;
  align-content: center;
  width: 100%;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.timeLine {
  position: absolute;
  height: 3px;
  width: 90.8%;
  margin-left: 9.2%;
  background-color: rgb(190, 4, 60);
  z-index: 5;
}

.grid {
  display: grid;
  width: 100%;
  align-content: flex-start;
  overflow-y: scroll;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.grid::-webkit-scrollbar {
  display: none;
}

.time {
  width: 100%;
  height: 60px;
  border: solid #555B6E;
  border-width: 1px;
  text-align: center;
  font-weight: bold;
  background-color: #78a1bb;
  overflow: hidden;
  color: white;
}

.blankRectangle {
  height: 30px;
}

.time h1 {
  line-height: 55px;
  font-size: 20px;
}

.weekDay {
  width: 100%;
  height: 30px;
  border: solid #555B6E;
  border-width: 1px;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  background-color: #78a1bb;
  overflow: hidden;
  color: white;
}

@media (max-width: 800px) {
  .time h1 {
    font-size: 16px;
  }
}

@media (max-width: 600px) {
  .time h1 {
    font-size: 10px;
  }
}
</style>