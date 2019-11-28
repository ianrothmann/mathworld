<template>
    <div>

        <div
                style="width: 1000px; height: 1000px; border-radius: 20px; box-shadow: 15px 10px 10px gray; text-align: center;"
                :style="{'background-color':colors[num]}" @click="blockClick()"
        >
            <span style="font-size: 50pt">
                {{names[num]}}
            </span>
        </div>


    </div>
</template>

<script>
  export default {
    name: "Ziane",
    data(){
      return {
       colors:[ 'deeppink','hotpink', 'deepskyblue'],
       names:['lam lam','fapfap','kattie'],
       num:0,
      };
    },
    methods:{
      blockClick(){
        this.num = this.num + 1;
        if(this.num > 2){
          this.num=0;
        }
      }
    }
  }
</script>

<style scoped>

</style>