<template>
  <div class="result">
    <p >Your reaction time is <span :style="timeColoring">{{time}} ms</span>, like a</p>
    <h1 :style="similarityColoring">{{similarities}}</h1>
  </div>
</template>

<script>
export default {
  name: "ResultBackDrop",
  props:{
    time:Number,
    similarities:String,
    resultType:String
  },
  data(){
    return {
      colorRange : {
        slowest:'#FF204E',
        normal:'#FFAF61',
        fast:'#4CCD99',
        veryfast: '#C3FF93',
        insane: '#94FFD8'
      }
    }
  },
  computed: {
    timeColoring(){
      if(this.time!==0 && this.resultType !== '-'){
        return {color: this.colorRange[this.resultType]};
      }
    },
    similarityColoring(){
      if(this.similarities !== '-' && this.resultType !== '-'){
        return {color: this.colorRange[this.resultType]};
      }
    }
  }
}
</script>

<style scoped>
.result{
  display: flex;
  flex-direction: row;
  column-gap: 10px;
  justify-content: center;
  align-items: center;
  color: white;
}

.result span{
  color: #C3FF93;
}
.result h1{
  text-transform: uppercase;
}
</style>