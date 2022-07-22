<script>
export default {
  props: { peopleList: Array, selectedIndex: Number },
  data() {
    return {
      showSettings: false,
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
    <div>
      <h2 @click="$emit('personChange', -1)" class="nextName">
        {{ findNeighbour(-1) }}
      </h2>
    </div>
    <div>
      <h1 @click="showSettings = !showSettings" class="currentName">{{ peopleList[selectedIndex].name }}</h1>
      <Transition name="slide">
        <div v-if="showSettings" class="settings">
          <h2>Selected days</h2>
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
        </div>
      </Transition>
    </div>
    <div>
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

.settings {
  position: absolute;
  z-index: 100;
  width: 100%;
  background-color: #ffaa00;
  border: solid #555b6e;
  border-width: 1px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(7, 14%);
  margin-left: 2%;
  overflow: hidden;
  margin-bottom: 10px;

}

.currentName {
  font-weight: 600;
  text-decoration: underline;
}

.currentName:hover {
  font-size: 35px;
  cursor: pointer;
}

.nextName:hover {
  margin: auto;
  cursor: pointer;
  font-size: 30px;
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
  margin: auto;

}

.regular-checkbox:checked {
  background-color: #e9ecee;
  border: 1px solid #adb8c0;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05), inset 15px 10px -12px rgba(255, 255, 255, 0.1);
  color: #99a1a7;
  margin: auto;
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
  .nameContainer h1 {
    font-size: 25px;
  }

  .nameContainer h2 {
    font-size: 15px;
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
  -moz-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  -webkit-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  -o-transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
  transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
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
