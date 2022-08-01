<script>
export default {
  props: {
    peopleList: Array,
    selectedIndex: Number,
    timeScale: Number,
    formattedTime: Array
  },
  data() {
    return {
      showSettings: false,
    }
  },
  computed: {
    getShiftTimeStartRange() {
      return this.peopleList[this.selectedIndex].shiftEnd / this.timeScale;
    },
    getBreakTimeStartRange() {
      if (this.peopleList[this.selectedIndex].breakEnd != null) {
        return this.peopleList[this.selectedIndex].breakEnd / this.timeScale;
      }
      return 24 / this.timeScale;
    }
  },
  methods: {
    findNeighbour(direction) {
      if (!this.peopleList[this.selectedIndex + direction]) {
        return this.peopleList[
          this.peopleList.length - (this.selectedIndex + direction) * direction
        ].name;
      }
      return this.peopleList[this.selectedIndex + direction].name;
    },
  },
};
</script>
<template>
  <div class="nameContainer">
    <div class="nameDiv">
      <h2 @click="$emit('personChange', -1)" class="nextName">
        {{ findNeighbour(-1) }}
      </h2>
    </div>
    <div class="nameDiv">
      <h1 @click="showSettings = !showSettings" class="currentName">{{ peopleList[selectedIndex].name }}</h1>
      <Transition name="slide">
        <!-- Nustatymų langelis. Start -->
        <div v-if="showSettings" class="settings">
          <h1>Selected days</h1>
          <!-- Darbo dienų pasirinkimas. Start -->
          <div class="grid">
            <p>M</p>
            <p>T</p>
            <p>W</p>
            <p>Th</p>
            <p>F</p>
            <p>S</p>
            <p>Sn</p>
            <template v-for="(bool, index) in peopleList[this.selectedIndex].workDays">
              <input class="regular-checkbox" type="checkbox" v-model="peopleList[this.selectedIndex].workDays[index]"
                :true-value="true" :false-value="false" />
            </template>
          </div>
          <!-- Darbo dienų pasirinkimas. End -->
          <!-- Darbo valandų pasirinkimas. Start -->
          <h1>Show time</h1>
          <label>From: </label>
          <select v-model.number="peopleList[this.selectedIndex].shiftStart">
            <template v-for="n in getShiftTimeStartRange">
              <option :value='n * timeScale - timeScale'>{{
                  formattedTime[n - 1]
              }}</option>
            </template>
          </select>
          <label> to: </label>
          <select v-model.number="peopleList[this.selectedIndex].shiftEnd">
            <template v-for="n in 24 / timeScale">
              <option v-if="n * timeScale > peopleList[this.selectedIndex].shiftStart" :value='n * timeScale'>{{
                  formattedTime[n]
              }}</option>
            </template>
          </select><br><br>
          <!-- Darbo valandų pasirinkimas. End -->
          <!-- Pertraukos pasirinkimas. Start -->
          <h1>Break time</h1>
          <label>Has break: </label>
          <input class="regular-checkbox" type="checkbox" v-model="peopleList[this.selectedIndex].hasBreak"
            :true-value="true" :false-value="false" /><br>
          <template v-if="peopleList[this.selectedIndex].hasBreak">
            <label>From: </label>
            <select v-model.number="peopleList[this.selectedIndex].breakStart">
              <template v-for="n in getBreakTimeStartRange">
                <option :value='n * timeScale - timeScale'>{{
                    formattedTime[n - 1]
                }}</option>
              </template>
            </select>
            <label> to: </label>
            <select v-model.number="peopleList[this.selectedIndex].breakEnd">
              <template v-for="n in 24 / timeScale">
                <option v-if="n * timeScale > peopleList[this.selectedIndex].breakStart" :value='n * timeScale'>{{
                    formattedTime[n]
                }}</option>
              </template>
            </select><br>
          </template>
        </div>
        <!-- Pertraukos pasirinkimas. End -->
        <!-- Nustatymų langelis. End  -->
      </Transition>
    </div>
    <div class="nameDiv">
      <h2 @click="$emit('personChange', 1)" class="nextName">
        {{ findNeighbour(1) }}
      </h2>
    </div>
  </div>
</template>
<style scoped>
.nameContainer {
  display: grid;
  grid-template-columns: repeat(3, 33.3%);
  text-align: center;
  align-items: center;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-color: #ffaa00;
  color: white;
  height: 60px;
}

select {
  width: 25%;
  font-size: 0.8rem;
}

.nameDiv {
  width: 60%;
  margin-left: 20%;
  background-color: #78a1bb;
  border: 1px solid #555b6e;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  border-radius: 20px;
}

.settings {
  position: absolute;
  z-index: 100;
  width: 250%;
  margin-left: -75%;
  background-color: #78a1bb;
  border: 1px solid #555b6e;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  border-radius: 20px;
  margin-top: 1px;
  padding-bottom: 6px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(7, 14%);
  margin-left: 2%;
  overflow: hidden;
  margin-bottom: 10px;

}

.currentName {
  font-weight: 700;
  text-decoration: underline;
  font-size: 28px;
}

.currentName:hover {
  cursor: pointer;
}

.nextName:hover {
  cursor: pointer
}

.regular-checkbox {
  -webkit-appearance: none;
  background-color: #fafafa;
  border: 1px solid #cacece;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  padding: 9px;
  border-radius: 10px;
  display: inline-block;
  position: relative;
  margin: auto;
}

.regular-checkbox:active,
.regular-checkbox:checked:active {
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px 1px 3px rgba(0, 0, 0, 0.1);

}

.regular-checkbox:checked {
  background-color: #e9ecee;
  border: 1px solid #adb8c0;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05), inset 15px 10px -12px rgba(255, 255, 255, 0.1);
  color: #99a1a7;
}

.regular-checkbox:checked:after {
  content: '\2714';
  font-size: 14px;
  position: absolute;
  top: 0px;
  left: 3px;
  color: #258cdb;
}

@media only screen and (max-width: 800px) {
  .nameDiv {
    width: 70%;
  }

  .nameContainer h1 {
    font-size: 15px;
  }

  .nameContainer h2 {
    font-size: 12px;
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
  -moz-transition-duration: 0.6s;
  -webkit-transition-duration: 0.6s;
  -o-transition-duration: 0.6s;
  transition-duration: 0.6s;
  -moz-transition-timing-function: ease-in;
  -webkit-transition-timing-function: ease-in;
  -o-transition-timing-function: ease-in;
  transition-timing-function: ease-in;
}

.slide-enter-to,
.slide-leave-from {
  max-height: 250px;
  overflow: hidden;
}

.slide-enter-from,
.slide-leave-to {
  overflow: hidden;
  max-height: 0;
}
</style>
