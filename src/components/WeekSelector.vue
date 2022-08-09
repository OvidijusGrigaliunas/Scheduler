<script>
export default {
    props: {weekRange: Array, person: Object},
    data() {
        return {
            showDateSel: false,
            yearRange: [],
            month: ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12'],
            dayFormatted: ['01', '02', '03', '04', '05', '06', '07', '08', '09'],
            selectedYear: new Date().getFullYear(),
            selectedMonth: new Date().getMonth() + 1,
            selectedDay: new Date().getDate(),
            showError: false
        }
    },
    watch: {
        // Jei pakeičiamas asmuo, paslepia tekstą error
        person: {
            handler() {
                this.showError = false;
            },
            deep: true
        }
    },
    created() {
        let yearLimit = new Date().getFullYear() + 2;
        for (let i = 2022; i <= yearLimit; i++) {
            this.yearRange.push(i);
        }
    },
    computed: {
        weekRangeFormat() {
            let result = this.weekRange[0].getFullYear() + "/";
            let month = this.weekRange[0].getMonth() + 1;
            let day = this.weekRange[0].getDate();
            // Su if nustatome ar reikia pridėti 0 pradžioje,
            // kad gautume yyyy/mm/dd datos formatą
            result = month < 10 ? `${result}0${month}/` : `${result}${month}/`;
            result = day < 10 ? `${result}0${day}` : `${result}${day}`;
            // antroji data
            result = `${result} - ${this.weekRange[6].getFullYear()}/`;
            month = this.weekRange[6].getMonth() + 1;
            day = this.weekRange[6].getDate();
            result = month < 10 ? `${result}0${month}/` : `${result}${month}/`;
            result = day < 10 ? `${result}0${day}` : `${result}${day}`;
            return result;
        },
        getMonthLength() {
            //ar keliamieji metai
            if (((this.selectedYear % 4 === 0) && (this.selectedYear % 100 !== 0)) || (this.selectedYear % 400 === 0)) {
                return [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
            }
            return [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
        }
    },
    methods: {
        dateChange() {
            let selectedDate = new Date(this.selectedYear, this.selectedMonth - 1, this.selectedDay);
            selectedDate = selectedDate.getDay() !== 0 ? selectedDate.getDay() - 1 : 6;
            // Tikriname ar tą dieną dirba darbuotojas
            if (this.person.workDays[selectedDate] === true) {
                this.showError = false;
                this.$emit('changeDate', this.selectedYear, this.selectedMonth, this.selectedDay);
            } else {
                this.showError = true;
            }
        }
    }
}

</script>
<template>
    <div class="selectionContainer">
        <p>
            <i class="arrow left" @click="$emit('weekChange', -7)"></i>
        </p>
        <div>
            <h1 @click="showDateSel = !showDateSel">{{ weekRangeFormat }}</h1>
            <Transition name="slide">
                <!-- Datos pasirinkimas. Start -->
                <!-- Metų pasirinkimas -->
                <div class="selectDay" v-if="showDateSel === true">
                    <h2> Select date</h2>
                    <select class="yearSelect" v-model.number="selectedYear">
                        <template :key="year" v-for="year in yearRange">
                            <option :value='year'>{{
                                    year
                                }}
                            </option>
                        </template>
                    </select>
                    <!-- Mėnesio pasirinkimas -->
                    <select class="monthDaySelect" v-model="selectedMonth">
                        <template :key="month" v-for="(month, index) in month">
                            <option :value='index + 1'>{{
                                    month
                                }}
                            </option>
                        </template>
                    </select>
                    <!-- Dienos pasirinkimas -->
                    <select class="monthDaySelect" v-model="selectedDay">
                        <template :key="day" v-for="day in getMonthLength[selectedMonth - 1]">
                            <option v-if="day > 9" :value='day'>{{
                                    day
                                }}
                            </option>
                            <option v-else :value='day'>{{
                                    dayFormatted[day - 1]
                                }}
                            </option>
                        </template>
                    </select><br>
                    <p v-show="showError">{{ person.name }} doesn't work on this day</p>
                    <button @click="dateChange">Change date</button>
                </div>
                <!-- Datos pasirinkimas. End -->
            </Transition>
        </div>
        <p>
            <i class="arrow right" @click="$emit('weekChange', 7)"></i>
        </p>
    </div>
</template>
<style scoped>
.selectionContainer {
    display: grid;
    grid-template-columns: repeat(3, 33.3%);
    height: 50px;
    width: 100%;
    text-align: center;
    background-color: #78a1bb;
    border: solid #555B6E;
    border-width: 0 0 1px 1px;
    color: white;
}

.selectDay {
    position: absolute;
    margin-left: -30%;
    width: 160%;
    z-index: 90;
    margin-top: 10px;
    background-color: #78a1bb;
    border: 1px solid #555b6e;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0 -15px 10px -12px rgba(0, 0, 0, 0.05);
    border-radius: 20px;
    padding: 5px;
}

.selectionContainer p {
    font-size: 20px;
}

h1:hover {
    cursor: pointer;
}

.selectionContainer h1 {
    font-size: 25px;
    margin-top: 15px;
    line-height: 20px;
}

.arrow {
    margin-top: 9px;
    border: solid white;
    border-width: 0 7px 7px 0;
    display: inline-block;
    padding: 12px;
    cursor: pointer;
}

.arrow:hover {
    border-width: 0 7px 7px 0;
    padding: 14px;
    margin-top: 7px;
}

.yearSelect {
    width: 40%;
}

.monthDaySelect {
    width: 20%;
}

button:hover {
    background-image: linear-gradient(to top, #ffaa00, #fff 70%);
}

.right {
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
}

.left {
    transform: rotate(135deg);
    -webkit-transform: rotate(135deg);
}

@media only screen and (max-width: 1200px) {
    .selectionContainer h1 {
        font-size: 25px;
        line-height: 20px;
        margin: auto;
    }
}

@media only screen and (max-width: 620px) {
    .selectionContainer h1 {
        font-size: 20px;
        line-height: 20px;
        margin: auto;
    }
}

@media only screen and (max-width: 500px) {
    .selectionContainer h1 {
        font-size: 15px;
        line-height: 15px;
        margin: auto;
    }
}

.slide-enter-active {
    -moz-transition-duration: 0.3s;
    -webkit-transition-duration: 0.3s;
    -o-transition-duration: 0.3s;
    transition-duration: 0.3s;
    -moz-transition-timing-function: ease-in;
    -webkit-transition-timing-function: ease-in;
    -o-transition-timing-function: ease-in;
    transition-timing-function: ease-in;
}

.slide-leave-active {
    -moz-transition-duration: 0.3s;
    -webkit-transition-duration: 0.3s;
    -o-transition-duration: 0.3s;
    transition-duration: 0.3s;
    -moz-transition-timing-function: ease-in;
    -webkit-transition-timing-function: ease-in;
    -o-transition-timing-function: ease-in;
    transition-timing-function: ease-in;
}

.slide-enter-to,
.slide-leave-from {
    max-height: 100px;
    overflow: hidden;
}

.slide-enter-from,
.slide-leave-to {
    overflow: hidden;
    max-height: 0;
}
</style>