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
  <div class="actions-box">
    <button @click="attackEnemy">ATTACK</button>
    <button :disabled="currentRound % 3" @click="superAttackEnemy">SUPER ATTACK</button>
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
      currentRound: 0
    }
  },
  computed: {
    enemyBarStyle() {
      return {width: this.enemyHealth + '%'}
    },
    playerBarStyle() {
      return {width: this.playerHealth + '%'}
    }
  },
  methods: {
    attackEnemy() {
      this.currentRound++
      this.enemyHealth -= this.generateRandomDamage(5, 12)
      this.attackPlayer()
    },
    attackPlayer() {
      this.playerHealth -= this.generateRandomDamage(8, 15)
    },
    superAttackEnemy() {
      this.currentRound++
      this.enemyHealth -= this.generateRandomDamage(10, 25)
      this.attackPlayer()
    },
    healPlayer() {
      if (this.playerHealth < 100)
      this.playerHealth += 10;
    },
    generateRandomDamage(min, max) {
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
button:disabled {
  opacity: 0.3;
}
</style>
