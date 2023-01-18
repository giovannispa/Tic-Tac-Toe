<template>
  <div class="container mx-auto flex justify-center">
    <h1 v-if="winner" class="text-5xl">Winner: {{ winner }}</h1>
    <h1 v-else class="text-5xl">Player: {{ currentPlayer }}</h1>
  </div>
  <base-container v-for="(row, rowIndex) in board">
    <box v-for="(cell, cellIndex) in row" @click="handleMove(rowIndex, cellIndex)">
      <Item>
        {{ cell }}
      </Item>
    </box>
  </base-container>

  <div class="mx-auto flex justify-center">
    <button class="text-5xl px-4 py-2 font-semibold bg-cyan-500 text-white rounded-full shadow-sm" v-if="this.winner" @click="resetBoard">Reset Game</button>
  </div>
</template>

<script>
import BaseContainer from "./BaseContainer.vue";
import Box from "./Box.vue";
import Item from "./Item.vue";
import {ref, computed} from 'vue';

const checkWinner = (board) => {
  for(let i = 0; i < board.value.length; i++) {
    //Checando se houve vencedor em uma linha.
    if(board.value[i][0] === board.value[i][1] && board.value[i][1] === board.value[i][2]) {
      return board.value[i][0];
    }
    //Checando se houve vencedor em uma coluna.
    if(board.value[0][i] === board.value[1][i] && board.value[1][i] === board.value[2][i]) {
      return board.value[0][i];
    }
    //Checando de houve vencedor em diagonal.
    if(board.value[0][0] === board.value[1][1] && board.value[1][1] === board.value[2][2]) {
      return board.value[0][0];
    }
    if(board.value[0][2] === board.value[1][1] && board.value[1][1] === board.value[2][0]) {
      return board.value[0][2];
    }
  }
  return null;
};

export default {
  name: "TicTacToe",
  setup() {
    const board = ref([
      [null, null, null],
      [null, null, null],
      [null, null, null]
    ]);

    const currentPlayer = ref("X");

    const winner = computed(() => checkWinner(board));

    const handleMove = (row, cell) => {

      //Verificando se já existe um vencedor.
      if(winner.value) {
        return;
      }

      //Verificando se o campo clicado é válido.
      if(board.value[row][cell] === null) {
        //Atribuindo o simbolo clicado pelo player.
        board.value[row][cell] = currentPlayer.value;

        currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
      }
    };

    const resetBoard = () => {
      board.value = [
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ];
      currentPlayer.value = "X";
    }

    return { board, currentPlayer, winner, handleMove, resetBoard }
  },
  components: {BaseContainer, Box, Item}
}
</script>