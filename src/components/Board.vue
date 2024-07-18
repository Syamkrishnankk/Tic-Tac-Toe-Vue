<template>
  <v-app>
    <h1 align="center" class="mt-15 mb-10 text-h3">Tic-Tac-Toe</h1>
    
    <div class="gameboard" v-for="i in 3" align="center" v-if="!isWinner && !isTie">
      
      <v-dialog v-model="dialog" max-width="400" persistent >
      <template v-slot:activator="{ props: activatorProps }">
        
        <v-btn 
        v-for="j in 3"  
        class="" 
        v-bind="activatorProps" 
        @click="getQuestion(i-1,j-1)" 
        width="100" 
        height="100"
        rounded="0"
        variant="outlined"
        color="amber-lighten-5"
        >
        <!-- <h1>{{ checkedArray[i-1][j-1] }}</h1> -->
        <v-icon
        v-if="checkedArray[i-1][j-1] === 'O'"
          size="50"
          icon="mdi-minus-circle"
          color="amber-lighten-1"
          end
          class="mx-5"
        ></v-icon>
        <v-icon
        v-if="checkedArray[i-1][j-1] === 'X'"
          size="50"
          icon="mdi-checkbox-marked-circle"
          color="indigo-lighten-1"
          end
          class="mx-5"
        ></v-icon>
      </v-btn>
      </template>

      <v-card
        :text="inputQst"
        title="CSS Quiz"
        color="brown-darken-4"
      >
      <v-text-field class="mx-3" v-model="inputAnswer" @keyup.enter="dialog = false;getPosition()" ></v-text-field>
        <template v-slot:actions>
          <v-btn variant="tonal" @click="dialog = false;getPosition()" > Submit </v-btn>
        </template>
      </v-card>
    </v-dialog>
    </div>
    <div v-if="isWinner || isTie">
      <v-card class="mx-5 py-5" align="center" color="transparent">
        <h3 v-if="isWinner" class="text-h5 font-weight-medium"><span style="color: rgb(1, 231, 112);">{{ symbol== 'O'? 'Computer' : 'Player' }}</span> Win!!</h3>
        <h3 v-if="isTie" >Match Tie!!!</h3>
        <v-btn @click="reloadPage()" color="light-blue-darken-3" class="mt-5 btn">
          <p style="font-size: 12px;">Restart Game</p>
        </v-btn>
      </v-card>
    </div>
    <v-row justify="center" align="center">
      <p>Player : </p>
      <v-icon
        
          size="20"
          icon="mdi-checkbox-marked-circle"
          color="indigo-lighten-1"
          end
          class="mr-5"
        ></v-icon>
        <p>Computer : </p>
        <v-icon
       
          size="20"
          icon="mdi-minus-circle"
          color="amber-lighten-1"
          end
         
        ></v-icon>
    </v-row>
  </v-app>
</template>

<style scoped>
.btn{
  text-transform: none !important;
}

</style>

<script setup>
import { ref } from 'vue'
const dialog = ref(false)
const inputAnswer = ref('')
const isWinner = ref(false)
const isTie = ref(false)
const symbolPlayer = 'X'
const symbolComputer = 'O'
var iValue = ref();
var jValue = ref();
var inputQst = '';
const checkedArray = ref([["", "", ""], ["", "", ""], ["", "", ""]]);
var inputCheck = ref(false);
var qst = {};
var symbol = ref('');
var questions = ref({
  question : [{
    id : 1,
    question : "How to change the background colour of the text?",
    answer : "background-color"
  },
  {
    id : 2,
    question : "How to make the text appear in bold?",
    answer : "font-weight"
  },
  {
    id: 3,
    question: "How can I underline text?",
    answer: "text-decoration"
  },
  {
    id: 4,
    question: "What property controls the size of the text?",
    answer: "font-size"
  },
  {
    id: 5,
    question: "How to center text horizontally?",
    answer: "text-align"
  },
  {
    id: 6,
    question: "How can I add space between lines of text?",
    answer: "line-height"
  },
  {
    id: 7,
    question: "What property controls the font family (style) of the text?",
    answer: "font-family"
  },
  {
    id: 8,
    question: "How can I change the color of a link ?",
    answer: "color" 
  },
  {
    id: 9,
    question: "How to add a border around an image?",
    answer: "border" 
  }
  ]
})



const getQuestion = (i,j) =>{
  let askedQst = []
  iValue.value = i;
  jValue.value = j;
  console.log(iValue.value,jValue.value)
  console.log(questions.value.question.length)
  const randomIndex = Math.ceil(Math.random() * questions.value.question.length);
  if(askedQst.includes(randomIndex)){
    randomIndex = Math.ceil(Math.random() * questions.value.question.length);
    getQuestion();
  }else{
    qst = questions.value.question.find(
      (question) => question.id === randomIndex);
  inputQst = qst.question;
  }
  console.log(randomIndex)
  
  
  
  console.log(qst.question)

}
const getPosition= () =>{
  
  console.log(inputCheck.value)
  console.log(qst.answer)
  console.log(inputAnswer.value)
  if(qst.answer === inputAnswer.value.toLowerCase()){
    inputCheck.value = true;
    console.log('ansss')
    
  }
  if(inputCheck.value){
    console.log(inputCheck.value)
    if(checkedArray.value[iValue.value][jValue.value].length < 1){
      checkedArray.value[iValue.value][jValue.value] = symbolPlayer;
    }
  }
  
  if(!checkWinner() ){
    computerMove();
  }
  inputCheck.value = false;
  inputAnswer.value = '';
  qst = {}
}


const computerMove = () => {
  
  const emptyCells = [];
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (checkedArray.value[i][j] === "") {
        emptyCells.push({ row: i, col: j });
      }
    }
  }
  
  const randomIndex = Math.floor(Math.random() * emptyCells.length);
  checkedArray.value[emptyCells[randomIndex].row][emptyCells[randomIndex].col] = symbolComputer;
  checkWinner();
  
};

const checkWinner = () => {
  const winningConditions = [
    [0, 0, 1, 0, 2, 0],
    [0, 1, 1, 1, 2, 1],
    [0, 2, 1, 2, 2, 2],
    [0, 0, 0, 1, 0, 2],
    [1, 0, 1, 1, 1, 2],
    [2, 0, 2, 1, 2, 2],
    [0, 0, 1, 1, 2, 2],
    [0, 2, 1, 1, 2, 0]
  ]

  for (let condition of winningConditions) {
    const [a, b, c, d, e, f] = condition
    
    if (
      checkedArray.value[a][b] === checkedArray.value[c][d] &&
      checkedArray.value[a][b] === checkedArray.value[e][f] &&
      checkedArray.value[a][b] !== ""
    ) {
      console.log(checkedArray.value[a][b] + " Wins!")
      symbol.value = checkedArray.value[a][b];
      isWinner.value = true;
      console.log(checkedArray.value)
      return true
    }
  }
  let Tie = 0;
  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (checkedArray.value[i][j] === "") {
        Tie += 1;
        break;
      }
    }
  }

  if (Tie == 0) {
    isTie.value = true;
  }
}
const reloadPage = () =>{
  window.location.reload();
}
</script>