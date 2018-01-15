<template>
  <div id='board' class='container'>
    <template v-for="spell in deck">
      <card v-on:learnSpell="learnSpell" v-on:unLearnSpell="unLearnSpell" :namedCharacter="namedCharacter" :icons="icons" :spell="spell"></card>
    </template>
  </div>
</template>

<script>
import Card from './Card'

export default {
  name: 'board',
  components: {
    Card
  },
  props: ['namedCharacter'],
  data () {
    var list = require('../assets/spells.json')
    var icons = {}
    icons['Adivinación'] = require('../assets/adivination.svg')
    icons['Detectar'] = require('../assets/eye.svg')
    icons['Disipar magia'] = require('../assets/disipate.svg')
    icons['Disipar magia superior'] = require('../assets/disipate.svg')
    icons['Esencia animal maestra'] = require('../assets/claw.svg')
    icons['Esencia animal superior'] = require('../assets/claw.svg')
    icons['Esencia animal épica'] = require('../assets/claw.svg')
    icons['Esencia animal'] = require('../assets/claw.svg')
    icons['Esencia astral maestra'] = require('../assets/astral.svg')
    icons['Esencia astral superior'] = require('../assets/astral.svg')
    icons['Esencia astral'] = require('../assets/astral.svg')
    icons['Esencia de aire'] = require('../assets/water2.svg')
    icons['Esencia de agua'] = require('../assets/water.svg')
    icons['Esencia de fuego maestra'] = require('../assets/fire.svg')
    icons['Esencia de fuego'] = require('../assets/fire.svg')
    icons['Esencia de luz'] = require('../assets/sun.svg')
    icons['Esencia de sombras maestra'] = require('../assets/shadow.svg')
    icons['Esencia de sombras superior'] = require('../assets/shadow.svg')
    icons['Esencia de sombras'] = require('../assets/shadow.svg')
    icons['Esencia de tierra'] = require('../assets/earth.svg')
    icons['Esencia de magma'] = require('../assets/magma.svg')
    icons['Esencia mágica maestra'] = require('../assets/magic.svg')
    icons['Esencia mágica superior'] = require('../assets/magic.svg')
    icons['Esencia mágica'] = require('../assets/magic.svg')
    icons['Esencia nigromántica maestra'] = require('../assets/skull.svg')
    icons['Esencia nigromántica'] = require('../assets/skull.svg')
    icons['Esencia vegetal maestra'] = require('../assets/tree.svg')
    icons['Esencia vegetal superior'] = require('../assets/tree.svg')
    icons['Esencia vegetal'] = require('../assets/tree.svg')
    icons['Imbuir de poder superior'] = require('../assets/air.svg')
    icons['Imbuir de poder'] = require('../assets/air.svg')
    icons['Infinito'] = require('../assets/infinity.svg')
    icons['Permanencia maestra'] = require('../assets/clock.svg')
    icons['Permanencia superior'] = require('../assets/clock.svg')
    icons['Permanencia'] = require('../assets/clock.svg')
    icons['Regeneración animal maestra'] = require('../assets/claw_reg.svg')
    icons['Regeneración astral superior'] = require('../assets/astral_reg.svg')
    icons['Regeneración vegetal maestra'] = require('../assets/tree_reg.svg')
    return {completeList: list.spellData, icons: icons}
  },
  computed: {
    deck: function () {
      var list = this.completeList
      if (this.namedCharacter.selectedMenuOption === 'lvlup') {
        list = this.completeList.filter(function (spell) {
          // hide insifucient points
          if (spell.Coste > this.points) return false
          // hide existing ones
          if (this.spells.find(function (elem) {
            return elem.Nombre === spell.Nombre
          }) !== undefined) return false
          // hide requisites not met
          var compare = this.spells.filter(function (n) {
            return spell.Requisitos.indexOf(n) > -1
          })
          if (compare.length !== spell.Requisitos.length) return false
          return true
        }, this.namedCharacter)
      } else if (this.namedCharacter.selectedMenuOption === 'pj') {
        list = this.completeList.filter(function (spell) {
          if (this.find(function (elem) {
            return elem.Nombre === spell.Nombre
          }) === undefined) return false
          return true
        }, this.namedCharacter.spells)
      }
      return list
    }
  },
  methods: {
    learnSpell: function (spell, event) {
      this.$emit('learnSpell', spell)
    },
    unLearnSpell: function (spell, event) {
      this.$emit('unLearnSpell', spell)
    }
  }
}

</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
.container{
  display: flex;
  flex-flow: wrap;
  justify-content: space-around;;
  margin: 0 -5px;
}
</style>
