<template>
  <header>
    <h1>VUE Enemy Slayer Game</h1>
  </header>
  <div class="player-box">
    <h1>Player Health: {{ playerHealth }}</h1>
    <div class="health-bar">
      <div class="health-bar-progress" :style="playerBarStyle"></div>
    </div>
  </div>
  <div class="enemy-box">
    <h1>Enemy Health: {{ enemyHealth }}</h1>
    <div class="health-bar">
      <div class="health-bar-progress" :style="enemyBarStyle"></div>
    </div>
  </div>
  <div class="game-status" v-if="winner">
    <h2>Game Over</h2>
    <h3>After {{ currentRound }} rounds</h3>
    <h3 v-if="winner === 'player'">You Won !</h3>
    <h3 v-else-if="winner === 'enemy'">You Lost !</h3>
    <h3 v-else>It's a DRAW !</h3>
    <button @click="resetGame">Start New Game</button>
  </div>
  <div class="actions-box">
    <button @click="attackEnemy">ATTACK</button>
    <button :disabled="superAttackActivate" @click="superAttackEnemy">SUPER ATTACK</button>
    <button @click="healPlayer">HEAL</button>
    <button @click="surrender">SURRENDER</button>
  </div>

</template>

<script>


export default {
  name: 'App',
  components: {},
  data() {
    return {
      playerHealth: 100,
      enemyHealth: 100,
      currentRound: 0,
      winner: null
    }
  },
  computed: {
    enemyBarStyle() {
      if (this.enemyHealth < 20)
        return {width: this.enemyHealth + '%', backgroundColor: 'crimson'}

      return {width: this.enemyHealth + '%'}
    },
    playerBarStyle() {
      if (this.playerHealth < 20)
        return {width: this.playerHealth + '%', backgroundColor: 'crimson'}

      return {width: this.playerHealth + '%'}
    },
    superAttackActivate() {
      return this.currentRound % 3 !== 0
    }
  },
  watch: {
    playerHealth(value) {
      if (value <= 0 && this.enemyHealth <= 0) {
        this.winner = 'draw'
        this.playerHealth = 0
        this.enemyHealth = 0
      } else if (value <= 0) {
        this.winner = 'enemy'
        this.playerHealth = 0
      }
    },
    enemyHealth(value) {
      if (value <= 0 && this.playerHealth <= 0) {
        this.winner = 'draw'
        this.enemyHealth = 0
        this.playerHealth = 0
      } else if (value <= 0) {
        this.winner = 'player'
        this.enemyHealth = 0
      }
    }
  },
  methods: {
    attackEnemy() {
      this.currentRound++
      this.enemyHealth -= this.generateRandomValue(5, 12)
      this.attackPlayer()
    },
    attackPlayer() {
      this.playerHealth -= this.generateRandomValue(8, 15)
    },
    superAttackEnemy() {
      this.currentRound++
      this.enemyHealth -= this.generateRandomValue(10, 25)
      this.attackPlayer()
    },
    healPlayer() {
      const healValue = this.generateRandomValue(10, 20)
      if (this.playerHealth + healValue > 100)
        this.playerHealth = 100
      else
        this.playerHealth += healValue

      this.attackPlayer() // still attacks you, when your healing in the turn
    },
    surrender() {
      this.winner = 'enemy'
    },
    resetGame() {
      this.winner = 0
      this.playerHealth = 100
      this.enemyHealth = 100
      this.currentRound = 0
    },
    generateRandomValue(min, max) {
      return Math.floor(Math.random() * (max - min)) + min
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}

header {
  background-color: #2c3e50;
  color: white;
  margin: 0;
  padding: 0;
}

h1 {
  margin: 0;
  padding: 30px;
}

.health-bar {
  border: 1px solid #2c3e50;
  height: 50px;
}

.health-bar-progress {
  background-color: #42b983;
  width: 100%;
  height: 50px;
}

.actions-box {
  padding: 40px;
}

button {
  background-color: #42b983;
  border: 1px solid #42b983;
  width: 200px;
  display: block;
  border-radius: 5px;
  padding: 10px 15px;
  margin: 10px auto;
  color: white;
}

button:hover {
  background-color: #2c3e50;
}

button:disabled {
  opacity: 0.3;
}

.game-status {
  border: 1px solid #2c3e50;
  margin: 30px auto 0 auto;
  width: 50%;
}

.player-box, .enemy-box {

}
</style>
