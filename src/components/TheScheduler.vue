<script setup>
import SchedulerItem from './SchedulerItem.vue'
import TaskEditor from './TaskEditor.vue'
import WeekSelector from './WeekSelector.vue';
import PersonSelector from './PersonSelector.vue';
</script>

<script>
export default {
    data() {
        return {
            taskArray: [
                // Jei rašo nuo 10:00 iki 15:00, tai reiškia , kad nuo 15:00 jau laisvas
                //{ id: 0, taskName: 'task2', taskDesc: 'task2 desc', taskDay: "2022/08/01", taskHourStart: 9, taskHourEnd: 10, taskTarget: 'Vardenis', taskColor: '#34ebc3', taskStatus: 'finished' },
            ],
            people: [
                {
                    name: "Vardenis",
                    shiftStart: 0,
                    shiftEnd: 24,
                    hasBreak: true,
                    breakStart: 12,
                    breakEnd: 13,
                    workDays: [true, true, true, true, true, true, true]
                },
                {
                    name: "Pavardenis",
                    shiftStart: 9,
                    shiftEnd: 19,
                    hasBreak: true,
                    breakStart: 12,
                    breakEnd: 13,
                    workDays: [true, true, true, true, true, false, false]
                },
                {
                    name: "Bonilla",
                    shiftStart: 10,
                    shiftEnd: 19,
                    hasBreak: true,
                    breakStart: 11,
                    breakEnd: 13,
                    workDays: [true, true, true, true, true, false, true]
                },
                {
                    name: "Hobbs",
                    shiftStart: 7,
                    shiftEnd: 16,
                    hasBreak: false,
                    breakStart: null,
                    breakEnd: null,
                    workDays: [true, false, true, true, true, false, false]
                }
            ],
            idStack: [],
            userTasksHashMap: {},
            taskInfoForItems: [],
            selectedHour: null,
            selectedDate: new Date(),
            selectedDay: new Date().getDay(),
            linePosition: 0,
            showLine: true,
            selectedPersonIndex: 0,
            windowHeight: window.screen.availHeight - 197,
            currentWeek: [],
            weekDayStrArr: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
            // Veikia su 0.25, 0.5 ir 1
            // Jei 0.25, laiko tarpai yra 15min
            // 0.5 - 30 min; 1 - 1h
            // su vertėmis didesnėmis negu 1 neveikia
            // Pagal viską turėtų veikti su 0.1 ir 0.2 (6min ir 12min), bet vertės gaunamos su daug skaičių po kablelio.
            // Turi veikti su 1/2^x vertėmis, tik reikėtų pataisyti laiko formativimą truputi
            // Jei timeScale yra 0 susidaro infinite loop
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
            let result = [];
            for (let i = 0; i < 7; i++) {
                if (this.people[this.selectedPersonIndex].workDays[i] === true) {
                    result.push(i);
                }
            }
            return result;
        },
        getWeekGridStyle() {
            let workDaysNumb = this.getWorkDaysNumb.length;
            return {gridTemplateColumns: '9.1% repeat(' + workDaysNumb + ',' + 91 / workDaysNumb + '%)'}
        },
        getGridStyle() {
            let workDaysNumb = this.getWorkDaysNumb.length;
            if (workDaysNumb > 0) {
                return {
                    height: this.getResolutionHeight + 'px',
                    gridTemplateColumns: '9.1% repeat(' + workDaysNumb + ',' + 91 / workDaysNumb + '%)'
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
            timeArray.push('00:00');
            return timeArray;
        },
        generatedTaskColumnForEditor() {
            let person = this.people[this.selectedPersonIndex].name;
            let arr = [];
            for (let [, index] of Object.entries(this.userTasksHashMap[person][this.selectedDateFormatting])) {
                arr.push(this.taskArray[index]);
            }
            return arr;
        }
    },
    created() {
        for (let i = 10000; i > 0; i--) {
            this.idStack.push(i);
        }
        // sugeneruoja 8928 tasks
        for (let i = 0; i < 96; i++) {
            for (let j = 1; j < 10; j++) {
                let taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/0${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Vardenis',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
                taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/0${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Pavardenis',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
                taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/0${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Hobbs',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
            }
            for (let j = 10; j < 31; j++) {
                let taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Vardenis',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
                taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Pavardenis',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
                taskID = this.idStack.pop();
                this.taskArray.push({
                    id: taskID,
                    taskName: 'taskm' + i,
                    taskDesc: 'task2 desc',
                    taskDay: `2022/08/0${j}`,
                    taskHourStart: i * this.timeScale,
                    taskHourEnd: this.timeScale + i * this.timeScale,
                    taskTarget: 'Hobbs',
                    taskColor: '#34ebc3',
                    taskStatus: 'ongoing'
                },);
            }
        }
        let minutes = new Date().getMinutes();
        if (minutes >= 30) {
            this.selectedHour = minutes >= 45 ? new Date().getHours() + 0.75 : new Date().getHours() + 0.5;
        } else {
            this.selectedHour = minutes <= 15 ? new Date().getHours() : new Date().getHours() + 0.25;
        }
        this.createWeek(this.selectedDate);
        this.generateHashDate();
        this.groupTasksByPersonAndDate();
        this.showCurTime();
        window.onresize = () => {
            this.windowHeight = window.screen.availHeight - 197;
        }
        setInterval(this.showCurTime, 15000);
    },
    watch: {
        people: {
            handler() {
                this.showCurTime();
            },
            deep: true
        }
    },
    methods: {
        showCurTime() {
            let lineCurrentTime = new Date();
            let end = this.getShiftTimeArray[this.selectedPersonIndex][this.getShiftTimeArray[this.selectedPersonIndex].length - 1] + this.timeScale;
            let start = this.getShiftTimeArray[this.selectedPersonIndex][0];
            let position = lineCurrentTime.getHours() + lineCurrentTime.getMinutes() / 60 + lineCurrentTime.getSeconds() / 3600;
            if (position < end && position > start) {
                position = (position - start) * 60 / this.timeScale;
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
            }
        },
        createNewTask(name, desc, startsAt, endsAt, color) {
            let taskid = this.idStack.pop();
            let object = {
                id: taskid,
                taskName: name,
                taskDesc: desc,
                taskDay: this.selectedDateFormatting,
                taskHourStart: startsAt,
                taskHourEnd: endsAt,
                taskTarget: this.people[this.selectedPersonIndex].name,
                taskColor: color,
                taskStatus: 'ongoing'
            }
            let person = this.people[this.selectedPersonIndex].name;
            let index;
            // jei taskid jau yra užregistruotas hashMap,
            // mes tiesiog naują objektą įterpiame į tą vietą
            if (this.userTasksHashMap[person][this.selectedDateFormatting][taskid]) {
                index = this.userTasksHashMap[person][this.selectedDateFormatting][taskid];
                this.taskArray[index] = object;
            } else {
                // o jei ne, užpushiname objektą į galą ir tada naudojame hashTask funkciją
                index = this.taskArray.length;
                this.taskArray.push(object);
                this.hashTask(person, this.selectedDateFormatting, taskid, index);
            }
            this.updateTaskInfoForItems(this.taskArray[index]);
        },
        deleteTask(taskToDeleteID) {
            let index = this.userTasksHashMap[this.people[this.selectedPersonIndex].name][this.selectedDateFormatting][taskToDeleteID];
            this.updateTaskInfoForItems(this.taskArray[index], true);
            this.taskArray[index] = {};
            this.idStack.push(taskToDeleteID);
        },
        // Padeda surandant index užduoties. Nebereikia loopinti per taskArray, kad jį rastume
        hashTask(person, date, taskID, index) {
            if (this.userTasksHashMap[person] && this.userTasksHashMap[person][date]) {
                this.userTasksHashMap[person][date][taskID] = index;
            } else {
                this.userTasksHashMap[person] = Object.assign({[date]: {}}, this.userTasksHashMap[person]);
                this.userTasksHashMap[person][date][taskID] = index;
            }
        },
        // Naudojama keičiant žmogų arba datą.
        // Kad nebūtų undefined, sukuriame tuščia datos raktą
        generateHashDate() {
            let person = this.people[this.selectedPersonIndex].name;
            let week = this.dateFormatting(this.currentWeek);
            for (let i = 0; i < 7; i++) {
                if (!this.userTasksHashMap[person] || !this.userTasksHashMap[person][week[i]]) {
                    this.userTasksHashMap[person] = Object.assign({[week[i]]: {}}, this.userTasksHashMap[person]);
                }
            }
        },
        editTask(taskToChangeID, name, desc, startsAt, endsAt, color) {
            let object = {
                id: taskToChangeID,
                taskName: name,
                taskDesc: desc,
                taskDay: this.selectedDateFormatting,
                taskHourStart: startsAt,
                taskHourEnd: endsAt,
                taskTarget: this.people[this.selectedPersonIndex].name,
                taskColor: color,
                taskStatus: 'ongoing'
            };
            let index = this.userTasksHashMap[this.people[this.selectedPersonIndex].name][this.selectedDateFormatting][taskToChangeID];
            this.updateTaskInfoForItems(this.taskArray[index], true);
            this.taskArray[index] = object;
            this.selectedHour = startsAt;
            this.updateTaskInfoForItems(this.taskArray[index]);
        },
        finishTask(taskToFinishID) {
            let index = this.userTasksHashMap[this.people[this.selectedPersonIndex].name][this.selectedDateFormatting][taskToFinishID];
            this.taskArray[index].taskStatus = 'finished';
            this.updateTaskInfoForItems(this.taskArray[index]);
        },
        timeSelection(dateIndex, hour) {
            this.selectedDay = dateIndex + 1;
            this.selectedHour = hour;
            this.selectedDate = this.currentWeek[dateIndex];
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
            let currentWeekDay = date.getDay() !== 0 ? date.getDay() : 7;
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
            this.generateHashDate();
            this.generateTaskInfoForItems();
        },
        changePerson(direction) {
            let newIndex = this.selectedPersonIndex + direction;
            // Jei išeiname iš masyvo ribų, pagal direction nustatome ar turime peršokti į masyvo
            // pradžią ar pabaigą. Jei direction -1 - pabaiga, jei 1 - pradžia;
            this.selectedPersonIndex = this.people[newIndex] ? newIndex : this.people.length - newIndex * direction;
            this.getSelectedHour();
            this.showCurTime();
            this.generateHashDate();
            this.generateTaskInfoForItems();
        },
        groupTasksByPersonAndDate() {
            for (let i = 0; i < this.taskArray.length; i++) {
                let person = this.taskArray[i].taskTarget;
                this.hashTask(person, this.taskArray[i].taskDay, this.taskArray[i].id, i);
            }
            this.generateTaskInfoForItems();
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
        // Sugeneruoja 2d array, pagal kurią galime žinoti ar langelis turi užduotį. Šios funckijos pagalba persiunčiame langeliams reikalinga info
        generateTaskInfoForItems() {
            let taskInfoArray = new Array(7).fill(null).map(() => new Array(this.getShiftTimeArray[this.selectedPersonIndex].length).fill({
                name: null,
                color: '#ffffff',
                status: null
            }));
            let person = this.people[this.selectedPersonIndex].name;
            let week = this.dateFormatting(this.currentWeek);
            for (let i = 0; i < 7; i++) {
                taskInfoArray[i] = this.generateColumnTaskInfo(this.userTasksHashMap[person][week[i]], taskInfoArray[i]);
            }
            this.taskInfoForItems = taskInfoArray;
        },
        // Atnaujina 2d array. Naudojama, kai yra užduoties duomenis yra keičiami (pvz.: delete, edit, create)
        updateTaskInfoForItems(task, deleted = false) {
            let taskHourEndIndex = (task.taskHourEnd - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
            let taskHourStartIndex = (task.taskHourStart - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
            if (deleted === false) {
                for (let i = taskHourStartIndex; i < taskHourEndIndex; i++) {
                    this.taskInfoForItems[this.selectedDay - 1][i] = {
                        name: task.taskName,
                        color: task.taskColor,
                        status: task.taskStatus
                    };
                }
            } else {
                for (let i = taskHourStartIndex; i < taskHourEndIndex; i++) {
                    this.taskInfoForItems[this.selectedDay - 1][i] = {name: null, color: '#ffffff', status: null};
                }
            }
        },
        generateColumnTaskInfo(taskArrayMap, taskInfoArray) {
            let taskArrayColumn = taskInfoArray;
            for (let [, index] of Object.entries(taskArrayMap)) {
                let task = this.taskArray[index];
                let taskHourEndIndex = (task.taskHourEnd - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
                let taskHourStartIndex = (task.taskHourStart - this.people[this.selectedPersonIndex].shiftStart) / this.timeScale;
                for (let i = taskHourStartIndex; i < taskHourEndIndex; i++) {
                    taskArrayColumn[i] = {name: task.taskName, color: task.taskColor, status: task.taskStatus};
                }
            }
            return taskArrayColumn;
        },
        changeDate(year, month, day) {
            this.selectedDate = new Date(year, month - 1, day);
            let weekDay = this.selectedDate.getDay();
            this.selectedDay = weekDay !== 0 ? weekDay : 7;
            this.createWeek(this.selectedDate);
            this.generateHashDate();
            this.generateTaskInfoForItems();
        }
    }
}
</script>

<template>
    <PersonSelector :peopleList='people' :selectedIndex='selectedPersonIndex' :timeScale='timeScale'
                    :formattedTime='formatTime' @personChange="changePerson"/>
    <div class="container">
        <div class="gridContainer">
            <div class="gridAndDateContainer">
                <div class="selectContainer">
                    <WeekSelector :weekRange='currentWeek' :person="people[selectedPersonIndex]"
                                  @weekChange="changeWeek" @changeDate="changeDate"/>
                </div>
                <div class="grid" :style="getWeekGridStyle">
                    <!-- Pirmoji grid juosta. Start -->
                    <div class="time blankRectangle"></div>
                    <template :key='index' v-for="(bool, index) in people[selectedPersonIndex].workDays">
                        <div v-if="bool === true" class="weekDay">
                            <p>{{ weekDayStrArr[index] }}</p>
                        </div>
                    </template>
                </div>
                <!-- Pirmoji grid juosta. End -->
                <!-- Užduočių grid. Start -->
                <div class="grid" :style="getGridStyle">
                    <div v-show="showLine" class="timeLine" :style="linePosition"></div>
                    <template :key='i' v-for='(i, hourIndex) in getShiftTimeArray[selectedPersonIndex]'>
                        <div class="time">
                            <h1>{{ formatTime[i / timeScale] }}</h1>
                        </div>
                        <template :key='dayIndex' v-for="dayIndex in getWorkDaysNumb">
                            <!-- Jei tuo metu nedirba, mums užtenka noWork -->
                            <SchedulerItem v-if="getBreakTimeArray[selectedPersonIndex].includes(i)" :noWork='true'/>
                            <SchedulerItem v-else :day="dayIndex" :hour="i"
                                           :class="{ selected: selectedDay === dayIndex + 1 && selectedHour === i }"
                                           :color='taskInfoForItems[dayIndex][hourIndex].color'
                                           :name="taskInfoForItems[dayIndex][hourIndex].name"
                                           @timeSelected="timeSelection"
                                           :status='taskInfoForItems[dayIndex][hourIndex].status'/>
                        </template>
                    </template>
                </div>
                <!-- Užduočių grid. End -->
            </div>
            <TaskEditor :resolution='getResolutionHeight' :date='selectedDateFormatting' :hour='selectedHour'
                        :tasks='generatedTaskColumnForEditor' :formattedTime='formatTime'
                        :breakTime='getBreakTimeArray[selectedPersonIndex]'
                        :shiftTime='getShiftTimeArray[selectedPersonIndex]'
                        :timeScale='timeScale' @NewTask="createNewTask" @finishTheTask="finishTask"
                        @taskDeletion="deleteTask"
                        @taskEdit='editTask'/>
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
    border: solid 1px #555B6E;
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

.selected {
    border: dashed 3px #555b6e;
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
    border: solid 1px #555B6E;
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
    border: solid 1px #555B6E;
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