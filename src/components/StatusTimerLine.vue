<template>
  <transition>
    <div :style="progressStyles" class="style-bar-line" />
  </transition>
</template>

<script>
import calculatingDurationTime from '../utils/calculatingDurationTime'
export default {
  props: {
    remainedTimeOfRound: {
      type: Number,
      default: null
    },
    currentStage: {
      type: Number,
      default: null
    },
    stages: {
      type: Array,
      default: null
    }
  },
  data(){
    return{
      i: 1,
      progressStyles: 0
    }
  },
  watch: { 
    remainedTimeOfRound() {
      // calculation width of progress bar depending on this.currentStage, this.stages[this.currentStage].type,
      // this.stages[this.currentStage].start and this.stages[this.currentStage].planned_end
      let calculate;
      if(this.currentStage !== -1 && this.stages[this.currentStage].type !== 'pause'){
        calculate = this.remainedTimeOfRound / calculatingDurationTime(this.stages[this.currentStage].start, this.stages[this.currentStage].planned_end )
      }
      else if (this.currentStage === -1){
        calculate = this.remainedTimeOfRound / ( this.remainedTimeOfRound + (this.i ++) )
      }
      else if(this.currentStage !== -1 && this.stages[this.currentStage].type === 'pause'){
        calculate = this.remainedTimeOfRound / calculatingDurationTime(this.stages[this.currentStage].start, this.stages[this.currentStage + 1].start )
      }
      this.progressStyles = `width : ${(100 - (calculate * 100))}%`
    }
  }
}

</script>

<style>
.style-bar-line{
    position: fixed;
    left: 0;
    top: 92px;
    height: 4px;
    background-color: #9ab913;
    transition: width .6s ease;
    z-index: 3;
}

@media screen and (max-height: 480px) {
  .style-bar-line{
    top: 50px;
  }
}

@media screen and (max-width: 478px) {
  .style-bar-line {
    top: 96px;
  }
}

</style>
