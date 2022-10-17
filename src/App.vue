<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'

import {ref, computed} from 'vue';


const player:{value:string} = ref('X');
const board:{value:any} = ref([['','',''],['','',''], ['','','']])
let step:{value:number} = ref(0)

const CalculateWinner = (squares:any)=>{
  const lines:Array<number[]> = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]];

  for (let i=0; i< lines.length; i++){
    const [a,b,c]= lines[i];
    if (squares[a] && squares[a]===squares[b] && squares[a]===squares[c]){
      return squares[a]
    }
  }
  return null;
};

const winner = computed(() => CalculateWinner(board.value.flat()));
const badgame = computed(()=>(step.value== 8)?true:false)
const MakeMove = (x:number|string,y:number) =>{
  
  if (winner.value) return;
  if (board.value[x][y] !== '') return;
  step.value++;

  board.value[x][y] = player.value;

  player.value =player.value=='X' ? 'O':'X';
  
  console.log("🚀 ~ file: App.vue ~ line 39 ~ MakeMove ~ step", step.value)
  
}

const ResetGame = () =>{
  board.value = [['','',''],['','',''], ['','','']];
  player.value ='X';
  step.value=0;
  
}

</script>
<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac</h1>
    <h3 class="text-xl mb-4">Player {{player}}'s turn</h3>
    <div class="flex flex-col item-center mb-8 justify-center">
      <div 
        class="flex item-center justify-center"
        v-for="(row, x) in board"
        :key="x"
      >
        <div 
          v-for = "(cell, y) in row"
          :key="y"
          @click="MakeMove(x,y)"
          :class="`border border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'X' ? 'text-pink-500' : 'text-blue-400'}`">
          {{ cell==='X' ? 'X' : cell=='O'?'O':''}}
          
        </div>
      </div>
    </div>
    <div class="text-center">
			<h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
      <h2 v-else-if ="badgame" class="text-6xl font-bold mb-8">Nobody Wins</h2>
			<button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300">Reset</button>
		</div>
  </main>
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}
</style>
