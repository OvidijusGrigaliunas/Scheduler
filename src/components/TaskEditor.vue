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
            tName: '',
            tDesc: '',
            selectedImportance: 3,
            selectedTimeStart: 5,
            selectedTimeEnd: 5,
            taskColor: '#ff7f50',
            showColorPallete: false,
            colorPallete: ['#ff7f50', '#87cefa', '#da70d6', '#32cd32', '#6495ed', '#ff69b4', '#ba55d3', '#cd5c5c', '#ffa500', '#40e0d0'],
            taskInfo: {}
        }
    },
    watch: {
        computedForWatcher() {
            this.findTask();
            this.selectedTimeStart = this.hour;
            this.selectedTimeEnd = this.hour + this.timeScale;
            this.showTaskEdit = false;
            this.tName = '';
            this.tDesc = '';
            this.taskColor = '#ff7f50';
        },
    },
    created() {
        this.findTask();
    },
    computed: {
        computedForWatcher() {
            return [this.date, this.hour, this.tasks];
        },
        hasTasksEditor() {
            this.showTaskEdit = false;
            return this.taskInfo.id != null;
        },
        getTaskHourStartEndLimit() {
            let lowestStart = this.shiftTime[0];
            let highestEnd = this.shiftTime[this.shiftTime.length - 1];
            let startingValue = this.taskInfo.taskHourStart;
            for (let i = 0; i < this.tasks.length; i++) {
                if (this.tasks[i].taskHourEnd < startingValue + this.timeScale) {
                    if (this.tasks[i].taskHourEnd > lowestStart) {
                        lowestStart = this.tasks[i].taskHourEnd;
                    }
                } else if (this.tasks[i].taskHourStart > startingValue) {
                    if (this.tasks[i].taskHourStart < highestEnd) {
                        highestEnd = this.tasks[i].taskHourStart - this.timeScale;
                    }
                }
            }
            return [lowestStart, highestEnd];
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
            timeCut[0].length = timeCut[0].indexOf(this.taskInfo.taskHourEnd) + 1;
            // Jei yra daugiau task juostoje prieš pasirinktą laiką,
            // mes vėl jį apkarpome nuo artimiausio task pabaigos
            if (this.getTaskHourStartEndLimit[0] != this.shiftTime[0]) {
                timeCut[0] = timeCut[0].slice(timeCut[0].indexOf(this.getTaskHourStartEndLimit[0]));
            }
            // Antroji dalis, praktiškai tas pats, tik apkarpome nuo priešingos pusės
            timeCut[1] = [...timeShiftWithoutBreaks];
            timeCut[1] = timeCut[1].slice(timeCut[1].indexOf(this.taskInfo.taskHourStart));
            if (this.getTaskHourStartEndLimit[1] != this.shiftTime[this.shiftTime.length - 1]) {
                timeCut[1].splice(timeCut[1].indexOf(this.getTaskHourStartEndLimit[1]) + 1, timeCut[1].length);
            }
            return timeCut;
        }
    },
    methods: {
        requirementsCheck() {
            if (this.tName.length === 0) {
                return false;
            }
            if (this.tDesc.length > 1024 || this.tName.length > 32) {
                return false;
            }
            if (this.selectedTimeStart >= this.selectedTimeEnd) {
                return false;
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
                this.$emit('taskEdit', this.taskInfo.id, this.tName, this.tDesc, this.selectedTimeStart, this.selectedTimeEnd, this.taskColor);
            }
        },
        findTask() {
            // Suranda užduotį pagal pasirinkto langelio laiką.
            let foundTask = this.tasks.find(task =>
                task.taskHourStart <= this.hour &&
                task.taskHourEnd - this.timeScale >= this.hour
            );
            let task = this.getTaskInfo(foundTask);
            if (JSON.stringify(this.taskInfo) !== JSON.stringify(task)) {
                this.taskInfo = task;
            }
        },
        showEditScreen() {
            // Nukopijuojame pradinės užduoties vertes, kad būtų patogiau keisti ją.
            this.showTaskEdit = true;
            this.tName = this.taskInfo.taskName;
            this.tDesc = this.taskInfo.taskDesc;
            this.selectedTimeStart = this.taskInfo.taskHourStart;
            this.selectedTimeEnd = this.taskInfo.taskHourEnd + this.timeScale;
            this.taskColor = this.taskInfo.taskColor;
        },
        getTaskInfo(task) {
            if (task) {
                return {
                    id: task.id,
                    taskName: task.taskName,
                    taskDesc: task.taskDesc,
                    taskHourStart: task.taskHourStart,
                    taskHourEnd: task.taskHourEnd - this.timeScale,
                    taskStatus: task.taskStatus,
                    taskColor: task.taskColor
                }
            }
            return {
                id: null,
                taskName: '',
                taskDesc: '',
                taskHourStart: this.hour,
                taskHourEnd: this.hour,
                taskStatus: '',
                taskColor: '#ff7f50'
            }
        },
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
            <button @click="showColorPallete = !showColorPallete" :style="{ background: taskColor }">Pick
                color</button><br>
            <!--- Spalvų paletė --->
            <div v-if="showColorPallete" class="colorPallete">
                <ul role="listbox">
                    <li v-for="color in colorPallete" @click="taskColor = color" :style="{ background: color }"
                        role="option">
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
                <h2 style="font-size: 40px">{{ taskInfo.taskName }}</h2>
                <!--- Užduoties pradžios - pabaigos laikas --->
                <h1>{{ formattedTime[taskInfo.taskHourStart / timeScale] }} - {{
                        formattedTime[taskInfo.taskHourEnd /
                        timeScale + 1]
                }}</h1>
                <!--- Užduoties būsena --->
                <h1>Status: {{ taskInfo.taskStatus }}</h1>
                <!--- Užduoties aprašymas --->
                <p>{{ taskInfo.taskDesc }}</p>
            </div>
            <!--- Informacija apie užduotį. End --->
            <template v-if="taskInfo.taskStatus === 'ongoing'">
                <button type="button" @click="$emit('finishTheTask', taskInfo.id)">Finish task</button><br><br>
                <button type="button" @click="showEditScreen()">Edit task</button><br><br>
            </template>
            <button type="button" @click="$emit('taskDeletion', taskInfo.id)">Delete task</button>
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