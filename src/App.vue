<template>
  <v-app>
    <v-app-bar>
      <v-img
        class="mx-2"
        src="./assets/logo.png"
        max-height="40"
        max-width="40"
        contain
      ></v-img>
      <v-toolbar-title class="title-style ml-2">
        Mind It!!
      </v-toolbar-title>
    </v-app-bar>
    <v-container class="text-center">
      <v-row v-for="(row, rowIndex) in numData" :key="rowIndex">
        <v-col v-for="(num, colIndex) in row" :key="colIndex">
          <Card
            :number="num"
            :row="rowIndex"
            :col="colIndex"
            :selectedCardIndices="selectedCardIndices"
            :hiddenCards="hiddenCards"
            @flipped="addMatch">
          </Card>
        </v-col>
      </v-row>
      <v-row class="justify-center">
        <v-col>
          <div class="score-style d-inline-flex">
            Score : {{score}}
          </div>
        </v-col>
        <v-col>
          <v-btn color="red" @click="resetGame()">Reset Game</v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-footer :padless="padless" class="justify-center" style="color: #E040FB">
      Designed and Coded by : surhud004 & Andrew Bastin &copy; 2020 <!-- Build Date : 8th June 2020 -->
    </v-footer>
  </v-app>
</template>
<style scoped>
  #app {
    font-family: 'Balsamiq Sans', cursive;
  }
  .title-style {
    color: #E040FB;
    font-weight: bold;
  }
  .score-style {
    height: 36px;
    padding-left: 16px;
    padding-right: 16px;
    padding-top: 0;
    padding-bottom: 0;
    background-color: #E040FB;
    font-weight: 500;
    font-size: 14px;
    letter-spacing: 0.0892857143em;
    text-transform: uppercase;
    border-radius: 4px;
    align-items: center;
  }
</style>
<script>
import Card from './components/Card'
export default {
  name: 'App',
  components: {
    Card
  },
  data () {
    return {
      score: 0,
      numData: [],
      selectedCardIndices: [],
      hiddenCards: [],
      allowSelection: true
    }
  },
  methods: {
    arrayEquals (a, b) {
      if (a === b) return true
      if (a === null || b === null) return false
      if (a.length !== b.length) return false

      for (var i = 0; i < a.length; ++i) {
        if (a[i] !== b[i]) return false
      }
      return true
    },
    startGame () {
      // Resetting the state, in case
      this.score = 0
      this.numData = []
      this.selectedCardIndices = []
      this.hiddenCards = []
      this.allowSelection = true

      var arr = []
      for (var i = 0; i < 26; i++) {
        const num = this.generateUniqueRandomNumber(arr)
        arr.push(num)
        arr.push(num)
      }
      const shuffled = this.shuffleArray(arr)

      const numData = [[], [], [], []]
      for (let i = 0; i < 52; i++) {
        numData[Math.floor(i / 13)].push(shuffled[51 - i])
      }
      this.numData = numData
    },
    generateUniqueRandomNumber (arr) {
      const num = Math.round(Math.random() * (99 - 1)) + 1
      if (arr.includes(num)) {
        return this.generateUniqueRandomNumber(arr)
      }
      return num
    },
    shuffleArray (array) {
      for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1))
        var temp = array[i]
        array[i] = array[j]
        array[j] = temp
      }
      return array
    },
    addMatch (ans, row, col) {
      if (this.allowSelection) {
        this.selectedCardIndices.push([row, col])
        if (this.selectedCardIndices.length === 2) {
          const [first, second] = this.selectedCardIndices
          if (!this.arrayEquals(first, second) && this.numData[first[0]][first[1]] === this.numData[second[0]][second[1]]) {
            this.allowSelection = false
            setTimeout(() => {
              this.score = this.score + 2
              this.selectedCardIndices = []
              this.hiddenCards = [...this.hiddenCards, first, second]
              this.allowSelection = true
            }, 1000)
          } else {
            this.allowSelection = false
            setTimeout(() => {
              // alert('wrong')
              this.selectedCardIndices = []
              this.allowSelection = true
            }, 1000)
          }
        }
      }
    },
    resetGame () {
      if (confirm('Are you sure you want to reset this game?')) {
        this.startGame()
      }
    }
  },
  created () {
    this.startGame()
  }
}
</script>
