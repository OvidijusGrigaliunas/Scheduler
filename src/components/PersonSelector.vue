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
      <div v-if="showSettings" class="settings">
        <template v-for="(bool, index) in peopleList[this.selectedIndex].workDays">
          <input class="regular-checkbox" type="checkbox" v-model="peopleList[this.selectedIndex].workDays[index]" :true-value="true"
            :false-value="false" />
        </template>
      </div>
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
  height: 200px;
  width: 100%;
  background-color: red;
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
  border-radius: 3px;
  display: inline-block;
  position: relative;
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
  color: #99a1a7;
}

@media only screen and (max-width: 800px) {
  .nameContainer h1 {
    font-size: 25px;
  }

  .nameContainer h2 {
    font-size: 15px;
  }
}
</style>
