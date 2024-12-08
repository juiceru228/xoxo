<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">

    <div v-if="showDialog" class="dialog">
      <h2>Выберите режим</h2>
      <button @click="startGameWithPC">Сыграть с ПК</button>
      <button @click="startGameWithFriend">Сыграть с другом</button>
    </div>
    <div v-if="showChooseSide" class="dialog">
      <h2>Выберите судьбу</h2>
      <button @click="chooseSide('X')">Играть за X</button>
      <button @click="chooseSide('0')">Играть за 0</button>
    </div>
    <div v-if="!showDialog && !showChooseSide">
      <div v-for="(row, index) in squares" :key="index" class="row">
        <div v-for="(col, jndex) in row" :key="jndex"
            class="frame" @click="toggleSquare(index, jndex)">
            {{ col >= "1" && col <= "9" ? "" : (col === "X" ? "X" : (col === "0" ? "0" : ""))  }}
        </div>
      </div>
      <button @click="resetGame"> Сброс игры</button>
      <button @click="changeGm"> Сменить режим</button>
    </div>
  </div>
</template>
<script>


export default  {
  name: 'App',
  data() {
    return {
      Z: "X",
      aiIndex: false,
      ai: false,
      showDialog: true,
      showChooseSide: false,
			index: "X",
			steps: 0,
      squares: [
        ["1", "2", "3"],
        ["4", "5", "6"],
        ["7", "8", "9"]
      ],
			squareStatus: [
			[false, false, false],
			[false, false, false],
			[false, false, false]
			]
    };
  },
  methods: {
    checkArr(){
      if(this.steps===9){
          alert("draw!");
          this.resetGame();
          return true;      
        }
        else if(this.squares[0][0] === this.squares[0][1] && this.squares[0][1] === this.squares[0][2] 
        || this.squares[1][0] === this.squares[1][1] && this.squares[1][1] === this.squares[1][2] 
        || this.squares[2][0] === this.squares[2][1] && this.squares[2][1] === this.squares[2][2] 
        || this.squares[0][0] === this.squares[1][0] && this.squares[1][0] === this.squares[2][0] 
        || this.squares[0][1] === this.squares[1][1] && this.squares[1][1] === this.squares[2][1] 
        || this.squares[0][2] === this.squares[1][2] && this.squares[1][2] === this.squares[2][2] 
        || this.squares[0][0] === this.squares[1][1] && this.squares[1][1] === this.squares[2][2]
        || this.squares[0][2] === this.squares[1][1] && this.squares[1][1] === this.squares[2][0])
        {
        this.index = this.index === "X" ? "0" : "X";
          alert(this.index + " player is won!");
          this.resetGame();
          return true; 
        }
        else
          return false;
    },
    aiTurn(){
      while(!this.aiIndex){
        this.rowIndex = Math.floor(Math.random() * 3);
        this.colIndex = Math.floor(Math.random() * 3);
        if(!this.squareStatus[this.rowIndex][this.colIndex] && this.steps < 9){
          this.squares[this.rowIndex][this.colIndex] = this.index === "X" ? "X" : "0";
          this.index = this.index === "X" ? "0" : "X";
          this.steps++;
          this.squareStatus[this.rowIndex][this.colIndex] = true;
          this.aiIndex = true;
          }
        }
        this.aiIndex = false;
    },
    changeGm(){
      this.resetGame();
      this.index = "X";
      this.showDialog = true;
      this.showChooseSide = false;
    },
    chooseSide(side){
      this.Z = side;
      this.index = side;
      if(this.Z === "0"){
        this.index = "X";
        this.aiTurn();
      }
      this.showChooseSide = false;
    },
    startGameWithPC(){
      this.showDialog = false;
      this.showChooseSide = true;
      this.ai = true;
    },
    startGameWithFriend(){
      this.showDialog = false;
      this.ai = false;
    },
    resetGame(){
      this.squares = [
        ["1", "2", "3"],
        ["4", "5", "6"],
        ["7", "8", "9"]
      ];
      this.squareStatus = [
			[false, false, false],
			[false, false, false],
			[false, false, false]
			];
      this.steps = 0;
      if(this.ai)
        this.showChooseSide = true;
      else
      this.index = "X";
    },
    toggleSquare(rowIndex, colIndex) {
      if(!this.squareStatus[rowIndex][colIndex] && this.steps < 9){
        this.squares[rowIndex][colIndex] = this.index === "X" ? "X" : "0";
        this.index = this.index === "X" ? "0" : "X";
        this.steps++;
        this.squareStatus[rowIndex][colIndex] = true;
        if(this.checkArr()){
          return;
        }
        if(this.ai){
          this.aiTurn();
        }    
        this.checkArr();
      } 
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.row {
  display: flex;
	margin: 0;
}
.frame {
	flex: 1;
	aspect-ratio: 1/1;
	border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 40px;
}
</style>
