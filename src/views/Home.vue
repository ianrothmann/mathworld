<template>
  <div style="width:100%; height: 100%">
    <div class="card" :style="{'background-color':wrong?'red':colors[i]}">
      <span>{{numerator}}</span>
      <span>÷</span>
      <span>{{divisor}}</span>
      <span>=</span>
    </div>
    <div style="margin: 50px">
      <v-btn @click="helpActive=!helpActive" color="error">Help</v-btn>
      <p style="margin: 20px; font-size:40px" v-if="helpActive">? x {{divisor}} = {{numerator}}</p>
    </div>


    <v-container style="max-width:200px;">
      <v-row>
        <v-col v-for="num in [1,2,3,4,5,6,7,8,9,10,0]" :key="num" cols="4">
          <v-btn color="primary" @click="giveAnswer(num)">
            {{num}}
          </v-btn>
        </v-col>
      </v-row>
    </v-container>

  </div>
</template>

<script>
// @ is an alias to /src
import {Howl} from 'howler';
export default {
  name: 'home',
  components: {
  },
  data(){
    return {
      numerator:null,
      divisor:null,
      answer : null,
      colors:['hotpink','deepskyblue','purple','deeppink'],
      i:0,
      helpActive:false,
      wrong:false,
      wrongSound : null,
      correctSound : null,
      includeRemainder : false,
      remainder : null
    }
  },
  mounted(){
    this.generate();

    this.wrongSound = new Howl({
      src: ['wrong.mp3'],
    });

    this.correctSound = new Howl({
      src: ['correct.mp3'],
    });
  },
  methods:{
    generate(){
       this.helpActive=false;
       this.answer = Math.ceil(Math.random()*10);
       this.divisor = Math.ceil(Math.random()*10);
       this.numerator = this.answer * this.divisor;

       if(this.includeRemainder){
         this.remainder = Math.floor(Math.random() * this.divisor - 1) + 1;
         this.numerator+=this.remainder;
       }

       if(this.i>=this.colors.length){
         this.i=0;
       }else{
         this.i++;
       }
    },
    giveAnswer(num){
      if(num!==this.answer){
        this.answerWrong();
      }else{
        this.answerCorrect();
        this.generate();
      }
    },
    answerWrong(){
      this.wrong=true;
      this.wrongSound.play();
      setTimeout(()=>{
        this.wrong=false;
      },1000);
    },
    answerCorrect(){
      this.correctSound.play();
    }
  }
}
</script>
<style scoped>
  .card{
    position: relative;
    background-color: deeppink;
    width: 800px;
    height: 300px;
    margin: 50px;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 10px 10px 10px #ccc;
    color: white;
    font-size: 150px;
    line-height: 250px;
  }

  .card span{
    margin:10px;
  }
</style>
