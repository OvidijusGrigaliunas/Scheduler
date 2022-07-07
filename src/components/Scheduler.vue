<script setup>
import SchedulerItem from './SchedulerItem.vue'
import Tasks from './Tasks.vue'
import WeekSelector from './WeekSelector.vue';
import PersonSelector from './PersonSelector.vue';

</script>

<script>

export default {
    data() {
        return {
            taskArray: [
            {taskName: 'task1',  taskDesc: 'task1 desc',  taskDay: "2022/07/05", taskHourStart: 7,    taskHourEnd: 9,   taskPerson: 'Vardenis',     taskImportance: 5},
            {taskName: 'task2',  taskDesc: 'task2 desc',  taskDay: "2022/07/05", taskHourStart: 3,    taskHourEnd: 5,   taskPerson: 'Vardenis',     taskImportance: 1},
            {taskName: 'task3',  taskDesc: 'task3 desc',  taskDay: "2022/06/29", taskHourStart: 0,    taskHourEnd: 9.5, taskPerson: 'Hobbs',        taskImportance: 2},
            {taskName: 'task4',  taskDesc: 'task4 desc',  taskDay: "2022/06/26", taskHourStart: 12,   taskHourEnd: 21,  taskPerson: 'Hobbs',        taskImportance: 5},
            {taskName: 'task5',  taskDesc: 'task5 desc',  taskDay: "2022/06/29", taskHourStart: 13,   taskHourEnd: 21,  taskPerson: 'Pavardenis',   taskImportance: 4},
            {taskName: 'task6',  taskDesc: 'task6 desc',  taskDay: "2022/07/10", taskHourStart: 8,    taskHourEnd: 10,  taskPerson: 'Vardenis',     taskImportance: 5},
            {taskName: 'task7',  taskDesc: 'task7 desc',  taskDay: "2022/06/27", taskHourStart: 10.5, taskHourEnd: 21,  taskPerson: 'Hobbs',        taskImportance: 4},
            {taskName: 'task8',  taskDesc: 'task8 desc',  taskDay: "2022/06/29", taskHourStart: 11,   taskHourEnd: 12,  taskPerson: 'Pavardenis',   taskImportance: 5},
            {taskName: 'task9',  taskDesc: 'task9 desc',  taskDay: "2022/06/29", taskHourStart: 8,    taskHourEnd: 21,  taskPerson: 'Bonilla',      taskImportance: 1},
            {taskName: 'task10', taskDesc: 'task10 desc', taskDay: "2022/06/29", taskHourStart: 0.5,  taskHourEnd: 3,   taskPerson: 'Pavardenis',   taskImportance: 3},
            {taskName: 'task11', taskDesc: 'task11 desc', taskDay: "2022/12/19", taskHourStart: 20,   taskHourEnd: 23,  taskPerson: 'Vardenis',     taskImportance: 1},
            {taskName: 'task12', taskDesc: 'task12 desc', taskDay: "2022/06/30", taskHourStart: 21,   taskHourEnd: 23,  taskPerson: 'Vardenis',     taskImportance: 2},
            {taskName: 'task13', taskDesc: 'task13 desc', taskDay: "2022/07/06", taskHourStart: 8,    taskHourEnd: 10,  taskPerson: 'Vardenis',     taskImportance: 3},
            {taskName: 'task14', taskDesc: 'task14 desc', taskDay: "2022/07/06", taskHourStart: 10.5, taskHourEnd: 13,  taskPerson: 'Vardenis',     taskImportance: 2},
            {taskName: 'task15', taskDesc: 'task15 desc', taskDay: "2022/07/07", taskHourStart: 6.5,  taskHourEnd: 13,  taskPerson: 'Vardenis',     taskImportance: 1},
            {taskName: 'task16', taskDesc: 'task16 desc', taskDay: "2022/07/08", taskHourStart: 8,    taskHourEnd: 8,   taskPerson: 'Vardenis',     taskImportance: 3},
            {taskName: 'task17', taskDesc: 'task17 desc', taskDay: "2022/07/08", taskHourStart: 9,    taskHourEnd: 13,  taskPerson: 'Vardenis',     taskImportance: 4},
            {taskName: 'task18', taskDesc: 'task18 desc', taskDay: "2022/07/09", taskHourStart: 5,    taskHourEnd: 20,  taskPerson: 'Vardenis',     taskImportance: 5},
            {taskName: 'task19', taskDesc: 'task19 desc', taskDay: "2022/07/08", taskHourStart: 0,    taskHourEnd: 2,   taskPerson: 'Vardenis',     taskImportance: 2},
            {taskName: 'task20', taskDesc: 'task20 desc', taskDay: "2022/07/10", taskHourStart: 5,    taskHourEnd: 7,   taskPerson: 'Vardenis',     taskImportance: 2},
            {taskName: 'task21', taskDesc: 'task21 desc', taskDay: "2022/07/08", taskHourStart: 2,    taskHourEnd: 5,   taskPerson: 'Vardenis',     taskImportance: 4},
            {taskName: 'task22', taskDesc: 'task22 desc', taskDay: "2022/07/09", taskHourStart: 2,    taskHourEnd: 4,   taskPerson: 'Vardenis',     taskImportance: 3},
            {taskName: 'task23', taskDesc: 'task23 desc', taskDay: "2022/07/04", taskHourStart: 2,    taskHourEnd: 5,   taskPerson: 'Vardenis',     taskImportance: 4},
            {taskName: 'task24', taskDesc: 'task24 desc', taskDay: "2022/07/04", taskHourStart: 9,    taskHourEnd: 12,  taskPerson: 'Vardenis',     taskImportance: 3},
            {taskName: 'task25', taskDesc: 'task25 desc', taskDay: "2022/07/08", taskHourStart: 2,    taskHourEnd: 5,   taskPerson: 'Pavardenis',   taskImportance: 4},
            {taskName: 'task26', taskDesc: 'task26 desc', taskDay: "2022/07/09", taskHourStart: 2,    taskHourEnd: 4,   taskPerson: 'Pavardenis',   taskImportance: 3},
            {taskName: 'task27', taskDesc: 'task27 desc', taskDay: "2022/07/04", taskHourStart: 2,    taskHourEnd: 5,   taskPerson: 'Pavardenis',   taskImportance: 4},
            {taskName: 'task28', taskDesc: 'task28 desc', taskDay: "2022/07/04", taskHourStart: 9,    taskHourEnd: 12,  taskPerson: 'Pavardenis',   taskImportance: 3},
            ],
            people: ["Vardenis", "Pavardenis", "Hobbs", "Bonilla"],
            selectedHour: 0,
            selectedDate: new Date(),
            selectedDay: new Date().getDay(),
            selectedPersonIndex: 0,
            gridAnimationRight: false,
            gridAnimationLeft: false,
            windowHeight: window.screen.availHeight - 197
        }
    },
    computed: {
        selectedDateFormatting() {
            let result = this.selectedDate.getFullYear() + "/"
            let month = this.selectedDate.getMonth() + 1;
            // Su if nustatome ar reikia pridėti 0 pradžioje,
            // kad gautume yyyy/mm/dd datos formatą
            if (month < 10) {
                result = result + 0 + month + "/";
            } else {
                result = result + month + "/";
            }
            if (this.selectedDate.getDate() < 10) {
                result = result + 0 + this.selectedDate.getDate()
            } else { 
                result = result + this.selectedDate.getDate();
            }
            return result;
        },
        getResolutionHeight() {
            let height = this.windowHeight;
            if(height < 750) {
                height = height + 85;
            } else if(height > 1000) {
                height = height + 85;
            }          
            return height;
        },
        setHeight(){
            let height = this.getResolutionHeight + 'px';
            return {height: height}
        },
        formatTime(){
            let timeArray = [];
            for(let i = 0; i < 10; i++) {
                timeArray.push('0' + i + ':00');
                timeArray.push('0' + i + ':30');
            }
            for(let i = 10; i < 24; i++) {
                timeArray.push(i + ':00');
                timeArray.push(i + ':30');
            }
            return timeArray;
        }
    },
    mounted() {
        window.onresize = () => {
            this.windowHeight = window.screen.availHeight - 197;
        }
    },
    methods: {
        createNewTask(name, desc, day, hour, importance, taskEndsAt) {
            let object = {
                taskName: name, 
                taskDesc: desc, 
                taskDay: day, 
                taskHourStart: hour,
                taskHourEnd: taskEndsAt, 
                taskPerson: this.people[this.selectedPersonIndex], 
                taskImportance: importance
            }
            this.taskArray.push(object);
        },
        deleteTask(taskToDelete) {   
            for (let i = 0; i < this.taskArray.length; i++) {
                if (taskToDelete[0] === this.taskArray[i]) {
                    this.taskArray.splice(i, 1);
                    break;
                }
            }
        },
        timeSelection(day, date, hour) {    
            this.selectedDay = day;
            this.selectedHour = hour;
            this.selectedDate = date;
        },
        addDays(date, days) { 
            let newDate = new Date(date);
            newDate.setDate(newDate.getDate() + days);
            return newDate;
        },
        // Sudarome savaitę pagal pasirinktą diena
        createWeek(date) {
            let selectedWeekDates = [];
            let currentWeekDay = date.getDay(); 
            // kai yra sekmadienis, vertė būna 0
            // tokiu atveju 0 reikia pasiversti į 7
            if (currentWeekDay === 0){
                currentWeekDay = 7;
            }
            for (let i = 1; i < 8; i++){
                // "i - currentWeekDay" reikalingas, kad savaitės dienos eitų iš eilės
                // (Monday, tuesday ir t.t.)
                selectedWeekDates.push(this.addDays(date, i - currentWeekDay))
            }
            return selectedWeekDates;
        },
        // Sutrumpinta createWeek funkcija
        // Naudojama jei reikalinga tik viena diena
        createWeekDay(date, day){
            let currentWeekDay = date.getDay(); 
            if (currentWeekDay === 0){
                currentWeekDay = 7;
            }
            return this.addDays(date, day - currentWeekDay);
        },
        changeWeek(direction) {
            this.animateGridHorizontal(direction);
            setTimeout(() => {
                this.selectedDate = this.addDays(this.selectedDate, direction);
            }, 150)
            
        },
        changePerson(direction) {
            this.selectedPersonIndex = this.selectedPersonIndex + direction
            // Jei išeiname iš masyvo ribų, pagal direction nustatome ar turime peršokti į masyvo
            // pradžią ar pabaigą. Jei direction -1 - pabaiga, jei 1 - pradžia;
            if(!this.people[this.selectedPersonIndex]) {
                this.selectedPersonIndex = this.people.length - (this.selectedPersonIndex * direction);
            }
        },
        animateGridHorizontal(direction) { 
            if (direction < 0) {
                this.gridAnimationLeft = true
            } else {
                this.gridAnimationRight = true;
            }
            setTimeout(() => {
                this.gridAnimationRight = false
                this.gridAnimationLeft = false
            }, 300)
        },
    }
  
}
</script>

<template>
    <PersonSelector :peopleList = 'people' :selectedIndex = 'selectedPersonIndex' @personChange="changePerson" />
    <div class = "container">
        <div class = "gridContainer">
            <div class = "gridAndDateContainer">
                <div class = "selectContainer" >
                    <WeekSelector :weekRange = 'createWeek(selectedDate)' @weekChange="changeWeek" />
                </div>
                <div class = "grid" :style = "setHeight" :class = "{slideRight: gridAnimationRight, slideLeft: gridAnimationLeft, }">
                    <div class = "time"></div>
                    <div class = "weekDay"> <p>Monday</p></div>
                    <div class = "weekDay"> <p>Tuesday</p></div>
                    <div class = "weekDay"> <p>Wednesday</p></div>
                    <div class = "weekDay"> <p>Thursday</p></div>
                    <div class = "weekDay"> <p>Friday</p></div>
                    <div class = "weekDay"> <p>Saturday</p></div>
                    <div class = "weekDay"> <p>Sunday</p></div>
                    <!--- 48 laiko tarpai 7 dienom.
                    (i - 1) reikalingas, kad laikas būtų nuo 00:00 iki 23:59.
                    Dalyba iš 2 reikalinga, kad gautume 30min laiko tarpus.--->
                    <template v-for = 'i in 48'>
                        <div class="time">
                            <h1>{{ formatTime[i - 1] }}</h1>
                        </div>
                        <template v-for = "n in 7">
                            <SchedulerItem 
                            v-if = "n === selectedDay && (i - 1) / 2 === selectedHour" 
                            class = "selected"
                            :day = 'n' 
                            :date = 'createWeekDay(selectedDate, n)' 
                            :hour = '(i - 1) / 2' 
                            :tasks = 'taskArray'
                            :name = 'people[selectedPersonIndex]' 
                            @timeSelected = "timeSelection" 
                            @taskDeletion = "deleteTask" />
                            <SchedulerItem 
                            v-else 
                            :day = 'n' 
                            :date = 'createWeekDay(selectedDate, n)'
                            :hour = '(i - 1) / 2'
                            :tasks = 'taskArray' 
                            :name = 'people[selectedPersonIndex]'
                            @timeSelected = "timeSelection"
                            @taskDeletion = "deleteTask" />
                        </template>
                    </template>
                </div>
            </div>
            <Tasks
            :resolution = 'getResolutionHeight'
            :day = 'selectedDay' 
            :date = 'selectedDateFormatting' 
            :tasks = 'taskArray' 
            :hour = 'selectedHour' 
            :name = 'people[selectedPersonIndex]'
            :formattedTime = 'formatTime'
            @NewTask = "createNewTask" 
            @taskDeletion = "deleteTask" />
        </div>
    </div>
</template>

<style scoped>

.gridContainer { 
    display: flex;
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Old versions of Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently */
    
}
.gridAndDateContainer {
    width: 70%;
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
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
    width:  100%;
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Old versions of Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently*/
}
.grid {
    display: grid;
    width: 100%;
    row-gap: 0;
    grid-template-columns: 9.1% repeat(7, 13%);
    overflow-y: scroll;
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  
}
.grid::-webkit-scrollbar {
    display: none;
}
.time{
    width: 100%;
    height: 30px;
    border: solid #555B6E;
    border-width: 1px;
    text-align: center;
    font-weight: bold;
    background-color: #78a1bb;
    overflow: hidden;
    color: white;
}
.time h1{
    line-height: 26px;
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
.selected {
    background-color: #277DA1;
}
.slideRight {
    animation: slideRight 0.3s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}
@keyframes slideRight {
  10%,30% {
    transform: translate3d(100%, 0, 0); 
  }
  40% {
   opacity: 0;  
  }
  50% {
    transform: translate3d(-200%, 0, 0);
  }
  60%, 100%{
   opacity: 1;
  }
}
.slideLeft {
    animation: slideLeft 0.3s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}
@keyframes slideLeft {
  10%,30% {
    transform: translate3d(-100%, 0, 0); 
  }
  40% {
   opacity: 0;  
  }
  50%{
    transform: translate3d(200%, 0, 0);
  }
  60%, 100%{
   opacity: 1;
  }
}

@media (max-width: 800px) {
    .time h1{
        line-height: 25px;
        font-size: 16px;
    }
}

@media (max-width: 600px) {
    .time h1{
        line-height: 25px;
        font-size: 10px;
    }
}

</style>