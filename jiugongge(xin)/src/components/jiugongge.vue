<template>
  <div class="hello">
          <div id="box">
            <div class="cless" ref='add' v-for= 'item in cells' @click = 'handleCellClick(item)' :key="item.id">{{item.content}}</div>
          </div>
          <div id="right">
            <div class="next">NEXT PLARER : <span id="play">{{this.player}}</span></div>
            <button id="start" @click = "resetCells">Go to game start</button>
            <div class="step" >
              <button :key="index" v-for="(item,index) in histories" @click='handleHistory(index)'>GO to move #{{index+1}}</button>
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
      winnerConditions:[
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 4, 8],
        [2, 4, 6],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
      ],
      cells:[],
      player:'X',
      winner:'',
      histories:[],
    }
  },
  mounted(){
    this.resetCells()
  },
  methods:{
    handleCellClick(item){
      if(item.content || this.winner){
        return
      }
      item.content = this.player;
      this.checkWinner();
      if(!this.winner){
        this.player = this.player == "X"?"O":"X";
        this.histories.push({id:item.id,content:item.content})
      }
      
    },
    handleHistory(index){
      const curHistory = this.histories.slice(0,index+1);
      this.cells.forEach((cell)=>{
        const hasValElem = curHistory.find((ch)=>ch.id === cell.id)
        cell.content = hasValElem ? hasValElem.content : ''
      })
    },
    checkWinner(){
      const sameCellIds = this.cells.filter((v)=>v.content === this.player).map((m)=>m.id);
      if(sameCellIds.length>2){
        const hasWinner = this.winnerConditions.some((v)=>v.every((m)=>sameCellIds.includes(m)));
        if(hasWinner){
          this.winner = this.player;
        }
      }
    },
    resetCells(){
      this.cells = [
        {id: 0,content :""},
        {id: 1,content :""},
        {id: 2,content :""},
        {id: 3,content :""},
        {id: 4,content :""},
        {id: 5,content :""},
        {id: 6,content :""},
        {id: 7,content :""},
        {id: 8,content :""},
      ];
      this.histories = [];
      this.winner = '';
      this.player = "X"

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
