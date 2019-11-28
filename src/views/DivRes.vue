<template>
  <div style="width:100%; height: 100%">
    <h2>Round {{roundNumber}}</h2>
    <div class="card" :style="{'background-color':wrong?'red':colors[i]}">
      <span>{{numerator}}</span>
      <span>÷</span>
      <span>{{divisor}}</span>
      <span>=</span>
      <span>{{partialAnswerDiv}}</span>
      <span v-if="partialAnswerDiv && remainder!==0">r</span>
      <span>{{partialAnswerRem}}</span>
    </div>
    <div style="margin: 50px">
      <v-btn @click="helpActive=!helpActive" color="error">Help</v-btn>
      <p style="margin: 20px; font-size:40px" v-if="helpActive">? x {{divisor}} = {{numerator - remainder}}</p>
      <p style="margin: 20px; font-size:40px" v-if="helpActive">{{numerator}} - {{numerator - remainder}} = ?</p>
    </div>


    <v-container>
      <v-row justify="center">
        <v-col cols="4" style="text-align: center"><h2>Antwoord</h2></v-col>
        <v-col cols="4" style="text-align: center"><h2>Res</h2></v-col>
      </v-row>
      <v-row justify="center">
        <v-col cols="4">
          <v-container style="max-width:200px;">
            <v-row>
              <v-col v-for="num in [1,2,3,4,5,6,7,8,9,10,0]" :key="num" cols="4">
                <v-btn color="primary" @click="giveAnswerDiv(num)">
                  {{num}}
                </v-btn>
              </v-col>
            </v-row>
          </v-container>
        </v-col>
        <v-col cols="4">
          <v-container style="max-width:200px;">
            <v-row>
              <v-col v-for="num in [1,2,3,4,5,6,7,8,9,10,0]" :key="num" cols="4">
                <v-btn color="primary" @click="giveAnswerRem(num)">
                  {{num}}
                </v-btn>
              </v-col>
            </v-row>
          </v-container>
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
      roundNumber:0,
      numerator:null,
      divisor:null,
      answer : null,
      colors:['hotpink','deepskyblue','purple','deeppink'],
      i:0,
      helpActive:false,
      wrong:false,
      wrongSound : null,
      correctSound : null,
      remainder : null,
      partialAnswerDiv : null,
      partialAnswerRem : null,
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
      if(this.helpActive){
        this.roundNumber+=0.5;
      }else{
        this.roundNumber++;
      }

      this.partialAnswerDiv=null;
      this.partialAnswerRem=null;
       this.helpActive=false;
       this.answer = Math.ceil(Math.random()*10);
       this.divisor = Math.ceil(Math.random()*10);
       this.numerator = this.answer * this.divisor;
       this.remainder = Math.floor(Math.random() * this.divisor - 1) + 1;
       this.numerator+=this.remainder;

       if(this.i>=this.colors.length){
         this.i=0;
       }else{
         this.i++;
       }
    },
    giveAnswerDiv(num){
      this.partialAnswerDiv=num;
      this.score();
    },
    giveAnswerRem(num){
      this.partialAnswerRem=num;
      this.score();
    },
    score(){
      if(this.partialAnswerDiv===null || (this.partialAnswerRem===null && this.remainder!==0)) return;

      if(this.partialAnswerDiv!==this.answer || (this.partialAnswerRem!==this.remainder && this.remainder!==0)){
        this.answerWrong();
      }else{
        this.answerCorrect();
      }

    },
    answerWrong(){
      this.wrong=true;
      this.wrongSound.play();
      setTimeout(()=>{
        this.wrong=false;
        this.partialAnswerDiv=null;
        this.partialAnswerRem=null;
      },1000);
    },
    answerCorrect(){
      this.correctSound.play();
      setTimeout(()=>{
        this.generate();
      },1000);
    }
  }
}
</script>
<style scoped>
  .card{
    position: relative;
    background-color: deeppink;
    width: 1000px;
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
