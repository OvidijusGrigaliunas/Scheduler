<script>
export default {
  props:['peopleList', 'selectedIndex'],
  computed: {
    
  },
  methods: {
    findNeighbour(direction) {        
        if(!this.peopleList[this.selectedIndex + direction]) {
            return this.peopleList[this.peopleList.length - ((this.selectedIndex + direction) * direction)].name;

        } else {
            return this.peopleList[this.selectedIndex + direction].name;
        }
    }
  }
}

</script>
<template>
    <div class = "nameContainer">
        <div class="nextName">
            <h2 @click = "$emit('personChange', -1)" class="nextName">{{ findNeighbour(-1) }}</h2>
        </div>
        <div class="currentName">
            <h1>{{ peopleList[selectedIndex].name }}</h1>
        </div>
        <div class="nextName">
            <h2 @click = "$emit('personChange', 1)" class="nextName">{{ findNeighbour(1) }}</h2>
        </div>
</div>
</template>
<style scoped>

.nameContainer{
    display: grid;
    grid-template-columns: repeat(3, 33.3%);
    text-align: center;
    align-items: center;
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Old versions of Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently*/
    background-color: #ffaa00;
    color: white;
    height: 60px;

}

.selectionContainer p {
    font-size: 30px;
    
}
.currentName h1{
    font-weight: 600;
    text-decoration: underline;
}
.nextName:hover{
    margin: auto;
    cursor: pointer;
    font-size: 30px;

}



@media only screen and (max-width: 800px) {
  .nameContainer h1{
    font-size: 25px;
  }
  .nameContainer h2{
    font-size: 15px;
  }

}

</style>