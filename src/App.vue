<script setup>
import { ref, computed } from "vue";
import Swal from "sweetalert2";

const player = ref("X");

const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculeteWinner = (board) => {
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
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
};

const winner = computed(() => CalculeteWinner(board.value.flat()));

const MakeMove = (x, y) => {
  if (winner.value) return;

  if (board.value[x][y]) return;

  board.value[x][y] = player.value;

  player.value = player.value === "X" ? "O" : "X";
};

const youWin = computed(() => {
  Swal.fire({
    icon: "success",
    title: `Good job  player ${winner.value}`,
    text: "Do you want to play again??",
    confirmButtonText: "Reset game",
  }).then(() => {
    ResetGame();
  });
});
const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`border border-white w-32 h-32 hover:bg-gray-700 flex items-center max-[900px]:w-24 max-[900px]:h-24
          justify-center material-icons-outlined text-6xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
      <h2 v-if="winner">
        {{ youWin() }}
      </h2>
    </div>

    <button
      v-if="winner == null"
      @click="ResetGame"
      class="px-4 py-2 bg-pink-500 rounded font-bold hover:bg-pink-600 duration-300"
    >
      Clean board
    </button>
  </main>
</template>
