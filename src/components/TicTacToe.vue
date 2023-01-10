<template>
  <div class="container mx-auto flex justify-center">
    <h1 class="text-5xl">Next player: {{ this.currentPlayer }}</h1>
  </div>
  <base-container v-for="(row, rowIndex) in board">
    <box v-for="(cell, cellIndex) in row" v-on:click="handleMove(rowIndex, cellIndex)">
      <Item>
        {{ cell }}
      </Item>
    </box>
  </base-container>
</template>

<script>
import BaseContainer from "./BaseContainer.vue";
import Box from "./Box.vue";
import Item from "./Item.vue";

export default {
  name: "TicTacToe",
  components: {Item, Box, BaseContainer},
  data() {
    return {
      board: [
        [null, null, null],
        [null, null, null],
        [null, null, null]
      ],
      currentPlayer: "X",
    }
  },
  methods: {
      resetBoard() {
        for(let i = 0; i < this.board.length; i++) {
          for(let j = 0; j < this.board[i].length; j++) {
            this.board[i][j] = null;
          }
        }
      },
      checkDraw(board) {
        let endGame = true;
        for(let i = 0; i < board.length; i++) {
          for(let j = 0; j < board[i].length; j++) {
            if(board[i][j] === null) {
              endGame = false;
            }
          }
        }

        return endGame;
      },
      checkWinner(board) {
        for(let i = 0; i < board.length; i++) {

          //Checando se houve vencedor em uma linha.
          if(board[i][0] === board[i][1] && board[i][1] === board[i][2]) {
            return board[i][0];
          }

          //Checando se houve vencedor em uma coluna.
          if(board[0][i] === board[1][i] && board[1][i] === board[2][i]) {
            return board[0][i];
          }

          //Checando de houve vencedor em diagonal.
          if(board[0][0] === board[1][1] && board[1][1] === board[2][2]) {
            return board[0][0];
          }

          if(board[0][2] === board[1][1] && board[1][1] === board[2][0]) {
            return board[0][2];
          }

        }

        return null;
      },
      handleMove(row, cell) {
        //Verificando se o campo clicado é válido.
        if(this.board[row][cell] === null) {

          //Atribuindo o simbolo clicado pelo player.
          this.board[row][cell] = this.currentPlayer;
        }

        //Checa se já á um vencedor.
        let winner = this.checkWinner(this.board);

        if(winner !== null) {
          //Avisando quem foi o vencedor.
          alert("Player " + winner + " is the winner!");

          //Zerando o tabuleiro.
          this.resetBoard();
        }

        //Checando se o jogo já acabou
        if(this.checkDraw(this.board)) {

          //Se o jogo acabou sem um vencedor, avisa o empate.
          alert("Draw!!!!!!!!!!");

          //Zerando o tabuleiro.
          this.resetBoard();
        }

        //Alterando para o próximo jogador.
        this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
      }
  }
}
</script>