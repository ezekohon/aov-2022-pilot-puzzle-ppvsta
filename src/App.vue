<template>
  <div class="w-full h-full flex flex-col justify-center items-center ">
    <p class="mb-2" v-if="!isGameFinished">{{ turnCaption }}</p>
    <p class="mb-2" v-else>{{ winnerCaption }}</p>
    <div class="mb-4">
      <Board :board="board" :is-player1-turn="isPlayer1Turn" :is-game-finished="isGameFinished"
        @square-click="handleSquareClick" />
    </div>
    <button class="border border-green bg-green text-white rounded-md px-4 py-2" :class="{ 'invisible': !isGameFinished }"
      @click="handleNewGame">Play
      again!</button>
  </div>

</template>

<script setup>
import { ref, computed } from 'vue'
import Board from "./components/Board.vue"

const board = ref([['', '', ''], ['', '', ''], ['', '', '']])

const winnerCaption = ref('')

const isPlayer1Turn = ref(true)

const isGameFinished = ref(false)

const turnNumber = ref(1)

const turnCaption = computed(() => isPlayer1Turn.value ? 'Player 1 turn: X' : 'Player 2 turn: O')

const handleSquareClick = (index) => {
  console.log(turnNumber.value);
  board.value[index.indexX][index.indexY] = isPlayer1Turn.value ? 'X' : 'O'
  const isWinner = isWinnerPattern(isPlayer1Turn.value ? 'X' : 'O')
  const isDraw = turnNumber.value === 9 && !isWinner
  if (isWinner) {
    isGameFinished.value = true
    winnerCaption.value = `Player ${isPlayer1Turn.value ? '1' : '2'} is the winner!`
  }
  else if (isDraw) {
    isGameFinished.value = true
    winnerCaption.value = "It's a draw."
  }
  else {
    isPlayer1Turn.value = !isPlayer1Turn.value
    turnNumber.value++
  }
}

const handleNewGame = () => {
  board.value = [['', '', ''], ['', '', ''], ['', '', '']]
  winnerCaption.value = ''
  isPlayer1Turn.value = true
  turnNumber.value = 1
  turnCaption.value = ''
  isGameFinished.value = false
}

const isWinnerPattern = (player) => {
  // vertical
  if ((board.value[0][0] === player && board.value[1][0] === player && board.value[2][0] === player) ||
    (board.value[0][1] === player && board.value[1][1] === player && board.value[2][1] === player) ||
    (board.value[0][2] === player && board.value[1][2] === player && board.value[2][2] === player)) {
    return true
  }
  // horizontal
  else if ((board.value[0][0] === player && board.value[0][1] === player && board.value[0][2] === player) ||
    (board.value[1][0] === player && board.value[1][1] === player && board.value[1][2] === player) ||
    (board.value[2][0] === player && board.value[2][1] === player && board.value[2][2] === player)) {
    return true
  }
  // diagonal
  else if ((board.value[0][0] === player && board.value[1][1] === player && board.value[2][2] === player) ||
    (board.value[2][0] === player && board.value[1][1] === player && board.value[0][2] === player)) {
    return true
  }
  else {
    return false
  }
}
</script>
