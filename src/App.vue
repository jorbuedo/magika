<template>
  <div id="app" v-bind:style="{ backgroundImage: 'url(' + background + ')'}">
    <navbar :namedCharacter="namedCharacter" v-on:setMenuOption="useMenuOption"></navbar>
    <board v-on:learnSpell="learnSpell" v-on:unLearnSpell="unLearnSpell" :namedCharacter="namedCharacter"></board>
  </div>
</template>

<script>
import Navbar from './components/Navbar'
import Board from './components/Board'

export default {
  name: 'app',
  components: {
    Navbar,
    Board
  },
  methods: {
    useMenuOption: function (selectedOption) {
      this.namedCharacter.selectedMenuOption = selectedOption
    },
    learnSpell: function (spell, event) {
      this.namedCharacter.points -= spell.Coste
      this.namedCharacter.spells.push(spell)
    },
    unLearnSpell: function (spell, event) {
      this.namedCharacter.points += spell.Coste
      this.namedCharacter.spells = this.namedCharacter.spells.filter(function (e) {
        return e.Nombre !== this
      }, spell.Nombre)
    }
  },
  data () {
    var background = require('assets/xv.png')
    var character = require('assets/character.json')
    var namedCharacter = {
      selectedMenuOption: 'all',
      points: character.sp,
      spells: character.spells,
      name: character.name
    }
    return {background: background, namedCharacter: namedCharacter}
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #78645a;
  position: absolute;
  min-height: 100%;
  min-width: 100%;
}
</style>
