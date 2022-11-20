<template>
    <div class="grid grid-cols-3">
        <div v-for="(row, indexX) in board">
            <Square v-for="(square, indexY) in row" :value="square" :is-player1-turn="isPlayer1Turn"
                :is-game-finished="isGameFinished" @on-click="handleSquareClick(indexX, indexY)"></Square>
        </div>
    </div>
</template>

<script setup lang="ts">
import Square from './Square.vue'
import { ref } from 'vue'

const props = defineProps({
    board: { type: Array, required: true },
    isPlayer1Turn: Boolean,
    isGameFinished: Boolean
})

const emit = defineEmits(['toggleTurn', 'squareClick'])

const handleSquareClick = (indexX, indexY) => {
    emit('squareClick', { indexX, indexY })
}

</script>

<style scoped>

</style>