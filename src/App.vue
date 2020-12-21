<template>
  <div id="app">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">YOU</h1>
            <div class="healthbar">
                <div
                class="healthbar text-center"
                style="background-color: green; margin: 0; color: white;"
                :style="{width: playerHealth + '%'}"
                >
                  <p>{{ playerHealth }}</p>
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar">
                <div
                class="healthbar text-center"
                style="background-color: green; margin: 0; color: white;"
                :style="{width: monsterHealth + '%'}"
                >
                  <p>{{ monsterHealth }}</p>
                </div>
            </div>
        </div>
    </section>
    <section class="row controls" v-if="!gameIsRunning">
        <div class="small-12 columns">
            <button id="start-game" @click="gameStart">START NEW GAME</button>
        </div>
    </section>
    <section class="row controls" v-else>
        <div class="small-12 columns">
            <button id="attack" @click="attack">ATTACK</button>
            <button id="special-attack" @click="specialAttack">SPECIAL ATTACK</button>
            <button id="heal" @click="heal">HEAL</button>
            <button id="give-up" @click="giveUp">GIVE UP</button>
        </div>
    </section>
    <section class="row log" v-if="turns.length > 0">
        <div class="small-12 columns">
            <ul>
                <li
                v-for="(turn, index) in turns" :key="index"
                :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}"
                >
                  {{ turn.text }}
                </li>
            </ul>
        </div>
    </section>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  // components: {
  //   HelloWorld
  // }
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false,
      turns: []
    }
  },
  methods: {
    gameStart() {
      this.gameIsRunning = true
      this.playerHealth = 100
      this.monsterHealth = 100
      this.turns = []
    },
    attack() {
      var damage = this.calculateDamage(3, 10)
      this.monsterHealth -= damage
      this.turns.unshift({
        isPlayer: true,
        text: 'Player hits Monster ' + damage
      })
      this.checkWin()

      damage = this.calculateDamage(5, 12)
      this.playerHealth -= damage
      this.turns.unshift({
        isPlayer: false,
        text: 'Monster hits Player ' + damage
      })
      this.checkWin()

    },
    specialAttack() {
      let damage = this.calculateDamage(10, 20)
      this.monsterHealth -= damage
      this.turns.unshift({
        isPlayer: true,
        text: 'Player hits Monster hard ' + damage
      })
      this.checkWin()

      damage = this.calculateDamage(7, 15)
      this.playerHealth -= damage
      this.turns.unshift({
        isPlayer: false,
        text: 'Monster hits Player hard ' + damage
      })
      this.checkWin()
    },
    heal() {
      if (this.playerHealth <= 90) {
        this.playerHealth += 10
      } else {
        this.playerHealth = 100
      }
      this.turns.unshift({
        isPlayer: true,
        text: 'Player heals 10'
      })
      if (this.checkWin()) {
        return
      }

      let damage = this.calculateDamage(8, 12)
      this.playerHealth -= damage
      this.turns.unshift({
        isPlayer: false,
        text: 'Monster hits Player ' + damage
      })
      if (this.checkWin()) {
        return
      }
    },
    giveUp() {
      this.gameIsRunning = false
      if (confirm('Are you sure you want to give up')) {
        this.gameIsRunning = false
      } else {
        this.gameIsRunning = true
      }
    },
    calculateDamage(min, max) {
      var damage = Math.max(Math.floor(Math.random() * max) + 1, min)
      return damage
    },
    checkWin() {
      if (this.monsterHealth <= 0) {
        alert("You Won!")
        this.gameIsRunning = false
        return
      }
      if (this.playerHealth <= 0) {
        alert("You Lost!")
        this.gameIsRunning = false
        return
      }
    }
  }
}
</script>

<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
