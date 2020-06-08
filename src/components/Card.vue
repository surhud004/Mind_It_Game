<template>
  <v-card elevation="4">
    <v-card-title
      :class="{ 'card-hidden': isHidden, 'card-selected': isSelected }"
      class="card-style justify-center"
      :style="randomColor"
      @click="isFlipped()"><span v-if="isSelected">{{number}}</span></v-card-title>
  </v-card>
</template>
<style scoped>
  .card-style {
    font-weight: bold;
    height: 120px;
    cursor: pointer;
  }
  .card-selected {
    background-color: purple !important;
  }
  .card-hidden {
    display: none;
  }
</style>
<script>
export default {
  props: ['number', 'row', 'col', 'selectedCardIndices', 'hiddenCards'],
  data () {
    return {
      backgroundColor: 'purple'
    }
  },
  computed: {
    isSelected () {
      for (const [selRow, selCol] of this.selectedCardIndices) {
        if (selRow === this.row && selCol === this.col) {
          return true
        }
      }
      return false
    },

    isHidden () {
      for (const [hidRow, hidCol] of this.hiddenCards) {
        if (hidRow === this.row && hidCol === this.col) return true
      }
      return false
    },

    randomColor () {
      var colors = ['#FF1493', '#74EE15', '#4DEEEA', '#FFE700', '#FF6800', '#FF355E', '#FD5B78', '#CCFF00', '#66FF66', '#FF00CC']
      var randomcolor = colors[Math.floor(Math.random() * colors.length)]
      return {
        backgroundColor: randomcolor
      }
    }
  },
  methods: {
    isFlipped () {
      this.$emit('flipped', this.number, this.row, this.col)
    }
  }
}
</script>
