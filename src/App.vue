<template>
  <div id="app">
    <h1>Tic-Tac-Toe Game</h1>

    <!-- Container to hold the inputs and board side by side -->
    <div class="game-container">
      <!-- Input fields for player names with labels -->
      <div class="player-inputs">
        <label>
          Player 1 Name (X):
          <input 
            v-model="player1Name" 
            placeholder="Enter Player 1 Name"
            @input="resetGameWithNewPlayers"
          />
        </label>
        <label>
          Player 2 Name (O):
          <input 
            v-model="player2Name" 
            placeholder="Enter Player 2 Name"
            @input="resetGameWithNewPlayers"
          />
        </label>
      </div>

      <!-- Board for the Tic-Tac-Toe game -->
      <div class="board">
        <div 
          v-for="(cell, index) in board" 
          :key="index" 
          class="cell" 
          @click="makeMove(index)">
          {{ cell }}
        </div>
      </div>
    </div>

    <!-- Display the result -->
    <div v-if="winner" class="result">
      🎉 {{ getWinnerName() }} Wins! 🎉
    </div>
    <div v-if="isDraw && !winner" class="result">
      🤝 It's a Draw! 🤝
    </div>

    <!-- Restart button -->
    <button @click="resetGame" class="reset-button">Restart Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      player1Name: '', // Player 1's name (X)
      player2Name: '', // Player 2's name (O)
      board: Array(9).fill(null), // Represents the 3x3 board (9 cells)
      currentPlayer: 'X', // Start with 'X'
      winner: null,
      isDraw: false
    };
  },
  methods: {
    makeMove(index) {
      // If the cell is already filled or there is a winner, return early
      if (this.board[index] || this.winner) return;

      // Place the current player's symbol in the clicked cell
      this.$set(this.board, index, this.currentPlayer);

      // Check for a winner after the move
      if (this.checkWinner()) {
        this.winner = this.currentPlayer;
      } else if (!this.board.includes(null)) {
        // If there are no empty cells and no winner, it's a draw
        this.isDraw = true;
      } else {
        // Switch to the other player
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      // All possible winning combinations
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6] // Diagonals
      ];

      // Check each combination to see if there's a winner
      for (let combo of winningCombinations) {
        const [a, b, c] = combo;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          return true;
        }
      }
      return false;
    },
    getWinnerName() {
      // Return the name of the winner based on currentPlayer
      return this.winner === 'X' ? this.player1Name || 'Player 1' : this.player2Name || 'Player 2';
    },
    resetGame() {
      // Reset the game state to start a new game
      this.board = Array(9).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
      this.isDraw = false;
    },
    resetGameWithNewPlayers() {
      // Reset the game whenever player names are changed
      this.resetGame();
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin-top: 40px;
}

/* Flexbox container to align inputs and board side by side */
.game-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 50px; /* Space between the inputs and the board */
  margin-bottom: 20px;
}

.player-inputs {
  display: flex;
  flex-direction: column;
  margin-right: 20px;
}

.player-inputs label {
  margin-bottom: 15px;
  font-weight: bold;
}

.player-inputs input {
  padding: 10px;
  margin-top: 5px;
  width: 200px;
  font-size: 1rem;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
  justify-content: center;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  cursor: pointer;
  border: 2px solid #333;
}

.cell:hover {
  background-color: #e0e0e0;
}

.result {
  font-size: 1.5rem;
  margin-bottom: 20px;
}

.reset-button {
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #3498db;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.reset-button:hover {
  background-color: #2980b9;
}
</style>
