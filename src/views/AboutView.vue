<template>
  <div id="app">
    <div class="status">{{ status }}</div>
    <button @click="resetBoard">Reset</button>
    <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
      <button
        v-for="(cell, cellIndex) in row"
        :key="cellIndex"
        class="square"
        @click="makeMove(rowIndex, cellIndex)"
        style="width:40px;height:40px;"
      >
        {{ cell }}
      </button>
    </div>
  </div>
</template>


<script>
export default {
  name: "App",
  data() {
    return {
      board: Array(3).fill().map(() => Array(3).fill(null)),
      currentPlayer: 'X',
      status: 'Next player: X',
      gameOver: false,
    };
  },
  methods: {
    makeMove(rowIndex, cellIndex) {
      if (!this.gameOver && !this.board[rowIndex][cellIndex]) {
        this.$set(this.board[rowIndex], cellIndex, this.currentPlayer);
        if (this.checkForWinner()) {
          this.status = `Player ${this.currentPlayer} has won!`;
          this.gameOver = true;
        } else if (this.isBoardFull()) {
          this.status = "It's a draw!";
          this.gameOver = true;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
          this.status = `Next player: ${this.currentPlayer}`;
        }
      }
    },
    resetBoard() {
      this.board = Array(3).fill().map(() => Array(3).fill(null));
      this.currentPlayer = 'X';
      this.status = 'Next player: X';
      this.gameOver = false;
    },
    checkForWinner() {
      const lines = [
        [[0, 0], [0, 1], [0, 2]],
        [[1, 0], [1, 1], [1, 2]],
        [[2, 0], [2, 1], [2, 2]],
        [[0, 0], [1, 0], [2, 0]],
        [[0, 1], [1, 1], [2, 1]],
        [[0, 2], [1, 2], [2, 2]],
        [[0, 0], [1, 1], [2, 2]],
        [[0, 2], [1, 1], [2, 0]],
      ];
      for (const line of lines) {
        const [a, b, c] = line;
        if (
          this.board[a[0]][a[1]] &&
          this.board[a[0]][a[1]] === this.board[b[0]][b[1]] &&
          this.board[a[0]][a[1]] === this.board[c[0]][c[1]]
        ) {
          return true;
        }
      }
      return false;
    },
    isBoardFull() {
      return this.board.every(row => row.every(cell => cell));
    }
  }
};
</script>

<style>
#app {
  text-align: center;
}

.row {
  margin-bottom: 5px;
}

.square {
  display: inline-block;
  margin-right: 5px;
}
</style>
