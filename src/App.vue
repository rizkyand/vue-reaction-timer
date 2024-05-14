<script>
  import ReactionBackdrop from "@/components/ReactionBackdrop.vue";
  import ResultBackDrop from "@/components/ResultBackDrop.vue";
  export default {
    name : 'App',
    components : {ReactionBackdrop, ResultBackDrop},
    data(){
      return {
        delay: null,
        isStarted: false,
        buttonTitle: 'Start',
        reactTime:0,
        lastReactTime:0,
        highestReactTime:0,
        similarity : '-',
        category: '-',
        animalReactionTimes : {
          housefly: {time:5, category:'insane'},
          mantisShrimp: {time:8, category:'insane'},
          dragonfly: {time:25, category:'veryfast'},
          mouse: {time:50, category:'veryfast'},
          cat: {time:70, category:'fast'},
          dog: {time:100, category:'fast'},
          humanAverage: {time:150, category:'normal'},
          humanAthlete: {time:100, category:'fast'},
          frog: {time:200, category:'normal'},
          elephant: {time:250, category:'normal'},
          seaTurtle: {time:300, category:'slowest'},
          sloth: {time:1000, category:'slowest'},
        },
      }
    },
    methods : {
      startGame() {
        this.reactTime = 0;
        this.similarity = '-';
        this.buttonTitle = 'Get ready...';
        this.isStarted = !this.isStarted;
        console.log(this.isStarted);
        this.delay = 1000 + Math.random() * 4000;
        setTimeout(() =>{
          this.isStarted = !this.isStarted;
          this.$refs.theGame.openModal();
          console.log(this.delay, this.isStarted);
          this.buttonTitle = 'Start'
        }, this.delay);
      },
      getReactionTime(times){
        this.reactTime = times;
        this.countScore();
        this.getSimilarity(times);
      },
      countScore(){
        this.lastReactTime = this.reactTime;
        if(this.highestReactTime !== 0){
          this.highestReactTime = this.reactTime < this.highestReactTime? this.reactTime : this.highestReactTime;
        }else{
          this.highestReactTime = this.reactTime;
        }
      },
      getSimilarity(inputTime){
        let closestAnimal = null;
        let closestTimeDifference = Infinity;
        let theCategory = null;

        for (const [animal, {time, category}] of Object.entries(this.animalReactionTimes)) {
          const timeDifference = Math.abs(time - inputTime);
          if (timeDifference < closestTimeDifference) {
            closestTimeDifference = timeDifference;
            closestAnimal = animal;
            theCategory = category;
          }
        }
        this.similarity = closestAnimal;
        this.category = theCategory;
      }
    }
  }

</script>

<template>
  <header>
    <div class="head-content">
      <h3 class="last-rec">Last Record: {{lastReactTime}} ms</h3>
      <h3 class="high-rec">Highest Record: {{highestReactTime}} ms</h3>
    </div>
  </header>
  <div class="container">
    <h1>Reaction Timer</h1>
    <br/>
    <ResultBackDrop :time="reactTime" :similarities="similarity" :result-type="category"/>
    <button class="btn-play" @click="startGame" :disabled="isStarted">{{ buttonTitle }}</button>
  </div>
    <ReactionBackdrop ref="theGame" @reaction-time="getReactionTime"/>
</template>

<style scoped>
  header{
    color: white;
  }


  .head-content{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    column-gap: 150px;
    align-items: center;
    font-family: "Dubai Medium";
  }
  .container{
    background: rgba( 255, 255, 255, 0.15 );
    box-shadow: 0 8px 32px 0 rgba( 255, 255, 255, 0.17 );
    backdrop-filter: blur( 2px );
    -webkit-backdrop-filter: blur( 2px );
    border-radius: 20px;
    height: 40%;
    width: 40%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 20px;
    padding: 20px;
    font-family: "Dubai Medium";
  }

  .container h1:nth-child(1){
    padding: 30px;
    color: white;
  }

  .btn-play{
    width: 40%;
    height: 50%;
    border-radius: 10px;
    border:none;
    padding: 10px;
    color: #344C64;
    background-color: white;
    transition: ease-in-out 0.1s;
    font-family: "Dubai Medium";
  }

  .btn-play:hover{
    scale: 1.02;
    box-shadow: 0 8px 32px 0 rgba( 255, 255, 255, 0.17 );
    backdrop-filter: blur( 2px );
    -webkit-backdrop-filter: blur( 2px );
    background: rgba( 255, 255, 255, 0.25 );
    color: white;
    cursor: pointer;
  }
  .btn-play:active{
    scale: 0.98;
  }

  .btn-play:disabled{
    box-shadow: 0 8px 32px 0 rgba( 255, 255, 255, 0.17 );
    backdrop-filter: blur( 2px );
    -webkit-backdrop-filter: blur( 2px );
    background: rgba( 255, 255, 255, 0.25 );
    color: white;
    cursor: not-allowed;
    scale: 0.98;
  }

</style>
