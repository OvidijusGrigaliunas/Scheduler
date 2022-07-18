<script>
export default {
  props: { weekRange: Array },
  computed: {
    weekRangeFormat() {
      let result = this.weekRange[0].getFullYear() + "/"
      let month = this.weekRange[0].getMonth() + 1;
      // Su if nustatome ar reikia pridėti 0 pradžioje,
      // kad gautume yyyy/mm/dd datos formatą
      if (month < 10) {
        result = result + 0 + month + "/";
      } else {
        result = result + month + "/";
      }
      if (this.weekRange[0].getDate() < 10) {
        result = result + 0 + this.weekRange[0].getDate();
      } else {
        result = result + this.weekRange[0].getDate();
      }
      // antroji data
      result = result + ' - ' + this.weekRange[6].getFullYear() + "/";
      month = this.weekRange[6].getMonth() + 1;
      if (month < 10) {
        result = result + 0 + month + "/";
      } else {
        result = result + month + "/";
      }
      if (this.weekRange[6].getDate() < 10) {
        result = result + 0 + this.weekRange[6].getDate();
      } else {
        result = result + this.weekRange[6].getDate();
      }
      return result;
    }
  },
  methods: {
  }
}

</script>
<template>
  <div class="selectionContainer">
    <p>
      <i class="arrow left" @click="$emit('weekChange', -7)"></i>
    </p>
    <h1>{{ weekRangeFormat }}</h1>
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

.selectionContainer p {
  font-size: 20px;
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