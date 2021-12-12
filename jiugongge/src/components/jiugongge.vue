<template>
  <div class="hello">
          <div id="box">
            <div class="cless" ref='add' v-for= '(item,index) in data1' v-on:click = 'dianji($event,item)' :key="index"></div>
          </div>
          <div id="right">
            <div class="next">NEXT PLARER : <span id="play">{{massage}}</span></div>
            <button id="start"  @click = 'clear'>Go to game start</button>
            <div class="step">
              <button v-for='(item1,index1) in arr3' v-on:click='btn($event,index1)' :key="index1">GO to move #{{index1+1}}</button>
            </div>
          </div>
  </div>
</template>

<script>
export default {
  name: 'jiugongge',
  props: {
    msg: String
  },
  data(){
    return{
       massage:'X',
            //赢的棋子位置
            winner:[[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 4, 8],[2, 4, 6],[0, 3, 6],[1, 4, 7],[2, 5, 8],],
            //记录点击的棋子为X的位置
            arr1:[],
            //记录点击的棋子为Ｏ的位置
            arr2:[], 
            //记录棋子X是否赢了
            //记录点击的次数
            arr3:[],
            winX :'',
            //记录棋子O是否赢了  
            winO:'',  
            data1:[0,1,2,3,4,5,6,7,8],
            //记录每次点击btn时对应的棋子位置
            index:'',
            curHistory:[],
            hasValElem:[],
            cur:''
    }
  },
  methods:{
    dianji:function(e,item){
              //点击添加对象：
              
              //判断棋子输赢
              if(this.arr1.length>=2 && this.arr2.length>=2){
                this.winX = this.winner.some((win)=>win.every((w)=>this.arr1.includes(w)))
                this.winO = this.winner.some((win)=>win.every((w)=>this.arr2.includes(w)))
                // winner.some((win)=>win.every((w)=>arr.includes(w)))
              }
                //当格子不为空时禁止点击
                if(e.target.innerHTML != ''|| (this.winX || this.winO) ){
                  return false
                }else{
                  this.arr3.push({id:item,cless:this.massage})
                  //点击格子，改变棋子
                  e.target.innerHTML = this.massage;
                  if(this.massage ==='X'){
                    //当显示的棋子为X时，记录其位置
                    this.arr1.push(item)
                    this.massage = 'O';
                    
                }else{
                  //当显示棋子为O时记录器位置
                    this.arr2.push(item)
                    this.massage = 'X'
                }
                }
            },
    //清空整个棋盘
            clear:function(){
                this.$refs.add[0].innerHTML = ""
                this.$refs.add[1].innerHTML = ""
                this.$refs.add[2].innerHTML = ""
                this.$refs.add[3].innerHTML = ""
                this.$refs.add[4].innerHTML = ""
                this.$refs.add[5].innerHTML = ""
                this.$refs.add[6].innerHTML = ""
                this.$refs.add[7].innerHTML = ""
                this.$refs.add[8].innerHTML = ""
                this.arr3 = []
                this.arr1=[]
                this.arr2=[]
                this.massage = 'X'
            },
    btn:function(e,item1){
              this.index = item1+1;
              console.log(this.index)
              this.curHistory = this.arr3.slice(this.index)
              console.log(this.curHistory)
              for(let i = 0;i<this.curHistory.length;i++){
                this.$refs.add[this.curHistory[i].id].innerHTML = ""
              }
            }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    * {
        margin: 0;
        padding: 0;
      } 
      #box {
        width: 150px;
        height: 150px;
        display: flex;
        flex-wrap: wrap;
        float: left;
      }
      #right {
        width: 150px;
        height: 150px;
        margin-left: 20px;
        float: left;
      }
      #box .cless {
        width: 50px;
        height: 50px;
        border: 1px solid #ccc;
        box-sizing: border-box;
        text-align: center;
        font-size: 30px;
      }
</style>
