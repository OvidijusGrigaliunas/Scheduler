<script setup>
import SchedulerItem from './SchedulerItem.vue'
import Tasks from './Tasks.vue'
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
                {taskName: 'task1',  taskDesc: 'task1 desc',  taskDay: "2022/07/05", taskHourStart: 9,     taskHourEnd: 10,     taskTarget: 'Vardenis',     taskImportance: 5},
                {taskName: 'task2',  taskDesc: 'task2 desc',  taskDay: "2022/07/05", taskHourStart: 10,    taskHourEnd: 15,     taskTarget: 'Vardenis',     taskImportance: 4},
                {taskName: 'task3',  taskDesc: 'task3 desc',  taskDay: "2022/07/06", taskHourStart: 11,    taskHourEnd: 15.5,   taskTarget: 'Vardenis',     taskImportance: 2},
                {taskName: 'task4',  taskDesc: 'task4 desc',  taskDay: "2022/07/07", taskHourStart: 11,    taskHourEnd: 15,  taskTarget: 'Vardenis',        taskImportance: 2},
            ],
            people: [
                {name: "Vardenis",  shiftStart: 8,  shiftEnd: 17, breakStart: 12, breakEnd: 13, workDays: [1, 2, 3, 4, 5]},
                {name: "Pavardenis",shiftStart: 9,  shiftEnd: 18, breakStart: 12, breakEnd: 13, workDays: [1, 2, 3, 4, 5, 6]}, 
                {name: "Bonilla",   shiftStart: 10, shiftEnd: 19, breakStart: 11, breakEnd: 13, workDays: [1, 2, 3, 7]}, 
                {name: "Hobbs",     shiftStart: 7,  shiftEnd: 16, breakStart: 10, breakEnd: 11, workDays: [1, 2, 3, 5, 6]}
            ],
            selectedHour: new Date().getHours(),
            selectedDate: new Date(),
            selectedDay: new Date().getDay(),
            selectedPersonIndex: 0,
            windowHeight: window.screen.availHeight - 197,
            // Veikia su 0.25, 0.5 ir 1
            // Jei 0.25, laiko tarpai yra 15min
            // 0.5 - 30 min; 1 - 1h
            // su vertėmis didesnėmis negu 1 neveikia
            // Pagal viską turėtų veikti su 0.1 ir 0.2 (6min ir 12min), bet vertės gaunamos su daug skaičių po kablelio,
            // todėl nėra gaunamos geros vertės.
            timeScale: 0.25
        }
    },
    computed: {
        getShiftTimeArray() {
            let result = [];
            this.people.forEach((person, index) => {
                result[index] = [];
                for (let i = person.shiftStart; i < person.shiftEnd; i = i + this.timeScale) {
                    result[index].push(i)
                }
            });
            return result;
        },
        getBreakTimeArray() {
            let result = [];
            this.people.forEach((person, index) => {
                result[index] = [];
                for (let i = person.breakStart; i < person.breakEnd; i = i + this.timeScale) {
                    result[index].push(i)
                }
            });
            return result;
        },
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
        setHeight() {
            let height = this.getResolutionHeight + 'px';
            return {height: height}
        },
        formatTime() {
            let timeArray = [];
            // Būtų įmanoma padaryti, kad pats vartotojas galėtų nustatyti
            // laiko tarpus
            let text;
            for(let i = 0; i < 10; i = i + this.timeScale) {
                text = ('0' + (i - ( i % 1)) + ':' + (60 * (i % 1)));
                if(text.length != 5){
                    text =  text.substring(0, 4) + 0 + text.substring(4);
                }
                timeArray.push(text)
            }
            for(let i = 10; i < 24; i = i + this.timeScale) {
                text = ((i - (i % 1)) + ':' + (60 * (i % 1)));
                if(text.length != 5){
                    text =  text.substring(0, 4) + 0 + text.substring(4);
                }
                timeArray.push(text)
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
        // Keičiant vartotoją dažnai pasirinkta valanda išeidavo iš darbo laiko ribų
        // Ši funkcija tai pataiso
        getSelectedHour(){
            if(
                !this.getShiftTimeArray[this.selectedPersonIndex].includes(this.selectedHour) ||
                this.getBreakTimeArray[this.selectedPersonIndex].includes(this.selectedHour)
            ) {
                this.selectedHour = this.people[this.selectedPersonIndex].shiftStart;
            }
        },
        createNewTask(name, desc, day, startsAt, importance, endsAt) {
            let object = {
                taskName: name, 
                taskDesc: desc, 
                taskDay: day, 
                taskHourStart: startsAt,
                taskHourEnd: endsAt, 
                taskTarget: this.people[this.selectedPersonIndex].name, 
                taskImportance: importance
            }
            this.selectedHour = startsAt;
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
            this.getSelectedHour();
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
            this.selectedDate = this.addDays(this.selectedDate, direction);
        },
        changePerson(direction) {
            this.selectedPersonIndex = this.selectedPersonIndex + direction
            // Jei išeiname iš masyvo ribų, pagal direction nustatome ar turime peršokti į masyvo
            // pradžią ar pabaigą. Jei direction -1 - pabaiga, jei 1 - pradžia;
            if(!this.people[this.selectedPersonIndex]) {
                this.selectedPersonIndex = this.people.length - (this.selectedPersonIndex * direction);
            }
            this.getSelectedHour();
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
                <div class = "grid" :style = "setHeight">
                    <div class = "time blankRectangle"></div>
                    <div class = "weekDay"> <p>Monday</p> </div>
                    <div class = "weekDay"> <p>Tuesday</p> </div>
                    <div class = "weekDay"> <p>Wednesday</p> </div>
                    <div class = "weekDay"> <p>Thursday</p> </div>
                    <div class = "weekDay"> <p>Friday</p> </div>
                    <div class = "weekDay"> <p>Saturday</p> </div>
                    <div class = "weekDay"> <p>Sunday</p> </div>
                    <template v-for = 'i in getShiftTimeArray[selectedPersonIndex]'>
                        <div :class = "{biggerTime: timeScale >= 1}" class="time">
                            <h1>{{ formatTime[parseInt(i / timeScale)] }}</h1>
                        </div>
                        <template v-for = "n in 7">
                            <SchedulerItem 
                            v-if = "n === selectedDay && i === selectedHour" 
                            class = "selected"
                            :day = 'n' 
                            :date = 'createWeekDay(selectedDate, n)' 
                            :hour = 'i' 
                            :tasks = 'taskArray'
                            :person = 'people[selectedPersonIndex]' 
                            :breakTime = 'getBreakTimeArray[selectedPersonIndex]'
                            :timeScale = 'timeScale'
                            @timeSelected = "timeSelection" 
                            @taskDeletion = "deleteTask" />
                            <SchedulerItem 
                            v-else 
                            :day = 'n' 
                            :date = 'createWeekDay(selectedDate, n)'
                            :hour = 'i'
                            :tasks = 'taskArray' 
                            :person = 'people[selectedPersonIndex]'
                            :breakTime = 'getBreakTimeArray[selectedPersonIndex]'
                            :timeScale = 'timeScale'
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
            :target = 'people[selectedPersonIndex].name'
            :formattedTime = 'formatTime'
            :breakTime = 'getBreakTimeArray[selectedPersonIndex]'
            :shiftTime = 'getShiftTimeArray[selectedPersonIndex]'
            :timeScale = 'timeScale'
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
    width: 100%;
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
.time {
    width: 100%;
    height: 50px;
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
.biggerTime{
    height: 150px;
}
.time h1 {
    line-height: 40px;
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

@media (max-width: 800px) {
    .time h1{
        line-height: 36px;
        font-size: 16px;
    }
}

@media (max-width: 600px) {
    .time h1{
        line-height: 30px;
        font-size: 10px;
    }
}

</style>