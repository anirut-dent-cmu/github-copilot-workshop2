<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return { winner: squares[a], line: lines[i] };
    }
  }
  return { winner: null, line: [] };
}

const gameResult = computed(() => calculateWinner(board.value.flat()))
const winner = computed(() => gameResult.value.winner)
const winningLine = computed(() => gameResult.value.line)

const makeMove = (x, y) => {
  if (winner.value) return
  if (board.value[x][y] !== '') return

  board.value[x][y] = player.value
  player.value = player.value === 'X' ? 'O' : 'X'
}

const resetGame = () => {
  player.value = 'X'
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ]
}

const isWinningCell = (x, y) => {
  if (!winner.value) return false
  const index = x * 3 + y
  return winningLine.value.includes(index)
}
</script>

<template>
  <main>
    <h1>Tic Tac Toe</h1>

    <h3>Player {{ player }}'s turn</h3>

    <div class="board">
      <div 
        v-for="(row, x) in board" 
        :key="x"
        class="row">
        <div 
          v-for="(cell, y) in row" 
          :key="y"
          @click="makeMove(x, y)"
          class="cell"
          :class="{ 'is-x': cell === 'X', 'is-o': cell === 'O', 'is-winning': isWinningCell(x, y) }">
          {{ cell }}
        </div>
      </div>
    </div>

    <h2 v-if="winner">Player '{{ winner }}' wins!</h2>

    <button @click="resetGame">Reset Game</button>
  </main>
</template>

<style>
body {
  background-color: #2c3e50;
  color: #ecf0f1;
  font-family: sans-serif;
  text-align: center;
}

main {
  padding-top: 2rem;
}

h1 {
  margin-bottom: 2rem;
  font-size: 2rem;
  text-transform: uppercase;
  font-weight: bold;
}

h3 {
  font-size: 1.25rem;
  margin-bottom: 1rem;
}

.board {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 2rem;
}

.row {
  display: flex;
}

.cell {
  border: 1px solid white;
  width: 5rem;
  height: 5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  cursor: pointer;
}

.cell:hover {
  background-color: #34495e;
}

.cell.is-x {
  color: #e74c3c;
}

.cell.is-o {
  color: #3498db;
}

.cell.is-winning {
  background-color: #27ae60;
}

h2 {
  font-size: 4rem;
  font-weight: bold;
  margin-bottom: 2rem;
}

button {
  padding: 0.5rem 1rem;
  background-color: #e74c3c;
  border-radius: 0.25rem;
  text-transform: uppercase;
  font-weight: bold;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #c0392b;
}
</style>