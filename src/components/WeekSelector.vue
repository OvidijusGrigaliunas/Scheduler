<script>
export default {
  props: { weekRange: Array },
  data() {
    return {
      showDateSel: false,
      yearRange: [],
      month: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
      monthLength: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
      selectedYear: new Date().getFullYear,
      selectedMonth: new Date().getMonth,
      selectedDay: new Date().getDate
    }
  },
  created() {
    let yearLimit = new Date();
    console.log(yearLimit)
    for (let i = 2022; i < yearLimit; i++) {
      this.yearRange.push(i)
    }
    console.log(this.yearRange)
  },
  computed: {
    weekRangeFormat() {
      let result = this.weekRange[0].getFullYear() + "/"
      let month = this.weekRange[0].getMonth() + 1;
      let day = this.weekRange[0].getDate()
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
      if (((this.selectedYear % 4 == 0) && (this.selectedYear % 100 != 0)) || (this.selectedYear % 400 == 0)) {
        return [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
      }
      return [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    }
  },
}

</script>
<template>
  <div class="selectionContainer">
    <p>
      <i class="arrow left" @click="$emit('weekChange', -7)"></i>
    </p>
    <div>
      <h1 @click="showDateSel = !showDateSel">{{ weekRangeFormat }}</h1>
      <div class="selectDay" v-if="showDateSel === true">
        <label for="tname">Select date</label><br>
        <select v-model.number="selectedYear">
          <template v-for="year in yearRange">
            <option :value='year'>{{
                year
            }}</option>
          </template>
        </select>
      </div>
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
  border-width: 0 0px 1px 1px;
  color: white;
}

.selectDay {
  position: absolute;
  margin-left: -50%;
  width: 200%;
  z-index: 90;
  background-color: red;
}

input {
  width: 80%;
  border: 1px solid;
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
}

.selectionContainer p {
  font-size: 20px;
}

h1:hover {
  cursor: pointer;
}

.selectionContainer h1 {
  font-size: 25px;
  margin: auto;
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
  }
}

@media only screen and (max-width: 620px) {
  .selectionContainer h1 {
    font-size: 20px;
    line-height: 20px;
  }
}

@media only screen and (max-width: 500px) {
  .selectionContainer h1 {
    font-size: 15px;
    line-height: 15px;
  }
}
</style>