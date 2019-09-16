<template>
  <v-container>
    <v-layout
    wrap
    >
    <v-flex mb-12>
    <section class="row">
      
        <v-flex xs6>
            <h1 class="text-center">YOU</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: green; margin: 0; color: white;"
                        :style="{width: playerHealth + '%'}">
                    {{ playerHealth }}
                </div>
            </div>
        </v-flex>
        <v-flex xs6>
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: green; margin: 0; color: white;"
                        :style="{width: monsterHealth + '%'}">
                    {{ monsterHealth }}
                </div>
            </div>
        </v-flex>
    </section>
    </v-flex>

    <v-flex xs12>
    <section class="row controls" v-if="!gameIsRunning">
        <div class="small-12 columns">
            <v-btn block color="#333333" dark @click="startGame">START NEW GAME</v-btn>
        </div>
    </section>

    <section class="row controls" v-else>
        <v-flex xs12>
            <v-btn rounded color="#ff7367" :ripple="{center: true, class: '#ff3f43' }" @click="attack">ATTACK</v-btn>
            <v-btn rounded color="#ffaf4f" :ripple="{center: true, class: '#ff9a2b' }" @click="specialAttack">SPECIAL ATTACK</v-btn>
            <v-btn rounded color="#aaffb0" :ripple="{center: true, class: '#76ff7e' }" @click="heal">HEAL</v-btn>
            <v-btn rounded color="#ffffff" :ripple="{center: true, class: '##c7c7c7' }" @click="giveUp">GIVE UP</v-btn>
        </v-flex>
    </section>
    </v-flex>

    <v-flex xs12>
    <section class="row log" v-if="turns.length > 0">
        <div class="small-12 columns">
            <ul>
                <li v-for="turn in turns"
                v-bind:key="turn.id"
                    :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}">
                    {{ turn.text }}
                </li>
            </ul>
        </div>
    </section>
    </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import {VContainer, VBtn, VLayout, VFlex } from 'vuetify/lib'
export default {
  name: 'App',
  components: {
    VContainer,
    VBtn,
    VLayout,
    VFlex,
  },
  data: function () {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false,
      turns: []
  }
    },
    methods: {
        startGame: function () {
            this.gameIsRunning = true;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            this.turns = [];
        },
        attack: function () {
            var damage = this.calculateDamage(3, 10);
            this.monsterHealth -= damage;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster for ' + damage
            });
            if (this.checkWin()) {
                return;
            }

            this.monsterAttacks();
        },
        specialAttack: function () {
            var damage = this.calculateDamage(10, 20);
            this.monsterHealth -= damage;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster hard for ' + damage
            });
            if (this.checkWin()) {
                return;
            }
            this.monsterAttacks();
        },
        heal: function () {
            if (this.playerHealth <= 90) {
                this.playerHealth += 10;
            } else {
                this.playerHealth = 100;
            }
            this.turns.unshift({
                isPlayer: true,
                text: 'Player heals for 10'
            });
            this.monsterAttacks();
        },
        giveUp: function () {
            this.gameIsRunning = false;
        },
        monsterAttacks: function() {
            var damage = this.calculateDamage(5, 12);
            this.playerHealth -= damage;
            this.checkWin();
            this.turns.unshift({
                isPlayer: false,
                text: 'Monster hits Player for ' + damage
            });
        },
        calculateDamage: function(min, max) {
            return Math.max(Math.floor(Math.random() * max) + 1, min);
        },
        checkWin: function() {
            if (this.monsterHealth <= 0) {
                if (confirm('You won! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            } else if (this.playerHealth <= 0) {
                if (confirm('You lost! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            }
            return false;
        }
  },
  ecosystem: [
      {
        text: 'vuetify-loader',
        href: 'https://github.com/vuetifyjs/vuetify-loader',
      },
      {
        text: 'github',
        href: 'https://github.com/vuetifyjs/vuetify',
      },
      {
        text: 'awesome-vuetify',
        href: 'https://github.com/vuetifyjs/awesome-vuetify',
      },
    ],
    importantLinks: [
      {
        text: 'Documentation',
        href: 'https://vuetifyjs.com',
      },
      {
        text: 'Chat',
        href: 'https://community.vuetifyjs.com',
      },
      {
        text: 'Made with Vuetify',
        href: 'https://madewithvuejs.com/vuetify',
      },
      {
        text: 'Twitter',
        href: 'https://twitter.com/vuetifyjs',
      },
      {
        text: 'Articles',
        href: 'https://medium.com/vuetify',
      },
    ],
    whatsNext: [
      {
        text: 'Explore components',
        href: 'https://vuetifyjs.com/components/api-explorer',
      },
      {
        text: 'Select a layout',
        href: 'https://vuetifyjs.com/layout/pre-defined',
      },
      {
        text: 'Frequently Asked Questions',
        href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
      },
    ],
  };
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
</style>