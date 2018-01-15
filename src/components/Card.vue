<template>
  <div class="item"
  v-on:click="isFlipped = !isFlipped"
  v-bind:class="{ flipped: isFlipped }"
  v-bind:style="{ order: order }">
  <div class="card">
    <div class="front" v-bind:style="{ background: '#433 url(' + background.wall + ')'}">
      <img class="learnSpell" v-on:click="learnSpell" :src="learnSpellIcon" v-if="learnSpellIcon !== null"></img>
      <img class="hearthstone" :src="background.front"></img>
      <img class="hearthstone" :src="epicness" v-if="epicness !== null"></img>
      <span class="coste">{{ spell.Coste }}</span>
      <div class="corner">
        <template v-for="req in spell.Requisitos">
          <img class="requisitos" :src="icons[req]" :title="req" :style="{ backgroundColor: reqColor(req) }"></img>
        </template>
      </div>
      <img v-if="icons[spell.Nombre] !== undefined" class="picture" :src="icons[spell.Nombre]" :title="spell.Nombre"></img>
      <span class="nombre">{{ spell.Nombre }}</span>
      <div class="conjuro">
        <span class="palabras">{{ spell.Palabras }}</span>
        <span class="gestos">{{ spell.Gestos }}</span>
      </div>
    </div>
    <div class="back">
      <img class="hearthstone_back" :src="background.back"></img>
      <span class="efectos">{{ spell.Efectos }}</span>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'card',
  props: ['spell', 'icons', 'namedCharacter'],
  data () {
    var background = {}
    background.front = require('assets/hearthstone.png')
    background.wall = require('assets/purty-wood.png')
    background.back = require('assets/hearthstone_back.png')
    return {
      isFlipped: false,
      background: background
    }
  },
  computed: {
    order: function () {
      return this.spell.Coste
    },
    learnSpellIcon: function () {
      var optIcon = null
      if (this.namedCharacter.selectedMenuOption === 'lvlup') {
        optIcon = require('assets/add.png')
      } else if (this.namedCharacter.selectedMenuOption === 'pj') {
        optIcon = require('assets/delete.png')
      }
      return optIcon
    },
    epicness: function () {
      var color = null
      if (this.spell.Coste > 12) {
        color = require('assets/epic.png')
      } else if (this.spell.Coste > 8) {
        return color
      } else if (this.spell.Coste > 5) {
        color = require('assets/superior.png')
      } else {
        color = require('assets/basic.png')
      }
      return color
    }
  },
  methods: {
    reqColor: function (requisite) {
      var color = '#000'
      if (requisite.match('épica')) {
        color = '#C57BDB'
      } else if (requisite.match('maestra')) {
        color = '#64B0ED'
      } else if (requisite.match('superior')) {
        color = '#98C17B'
      } else {
        color = '#A9A7BE'
      }
      if (requisite.match('Adivinación')) color = '#98C17B'
      if (requisite.match('Infinito')) color = '#64B0ED'
      return color
    },
    learnSpell: function (event) {
      event.stopPropagation()
      if (this.namedCharacter.selectedMenuOption === 'lvlup') {
        this.$emit('learnSpell', this.spell)
      } else if (this.namedCharacter.selectedMenuOption === 'pj') {
        this.$emit('unLearnSpell', this.spell)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.item, .card, .front, .back {
  height: 390px;
  width: 280px;
}

.item {
  margin: 15px 10px;
  perspective: 1000px;
  line-height: 1.07em;
}

.card {
  position: relative;
  transition: 0.6s;
  transform-style: preserve-3d;
}

.item.flipped .card {
  transform: rotateY(180deg);
}

.front, .back {
  backface-visibility: hidden;
  position: absolute;
  top: 0;
  left: 0;
  box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.1);
  transition: box-shadow .75s;
  border-radius: 18px;
}

.front:hover, .back:hover {
  box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.25);
}

.front {
  z-index: 2;
  transform: rotateY(0deg);
}

.back {
  transform: rotateY(180deg);
  display: table;
}

.hearthstone {
  position: absolute;
  top: -27px;
  left: -28px;
  height: 432px;
  width: 325px;
}

.hearthstone_back {
  position: absolute;
  top: -11px;
  left: -14px;
  height: 410px;
  width: 310px;
  z-index: -1;
}

.corner {
  position: absolute;
  display: flex;
  justify-content: flex-start;
  bottom: 1px;
  left: 25px;
}

.corner img {
  height: 20px;
  width: 20px;
  border-radius: 7px;
  padding:2px;
  border:2px solid #000;
  margin: 0 2px;
}

.coste {
  position: absolute;
  top: 12px;
  left: 24px;
  transform: translateX(-50%);
  font-size: 3em;
  font-weight: 600;
  color: white;
  -webkit-text-stroke: 2px black;
  text-shadow:
  -1px -1px 0 #000,
  1px -1px 0 #000,
  -1px 1px 0 #000,
  1px 1px 0 #000;
}

.requisitos {
}

.picture {
  position: absolute;
  top: 35px;
  height: 120px;
  width: 120px;
  margin: 0 auto;
  right: 0;
  left: 0;
}

.nombre {
  position: absolute;
  bottom: 50%;
  width: 85%;
  transform: translateY(50%);
  margin: 0 auto;
  left: 0;
  right: 0;
  font-size: 1.3em;
  font-weight: 600;
  color: white;
  -webkit-text-stroke: 1px black;
  text-shadow:
  -1px -1px 0 #000,
  1px -1px 0 #000,
  -1px 1px 0 #000,
  1px 1px 0 #000;
}

.conjuro {
  position: absolute;
  bottom: 16px;
  width: 70%;
  min-height: 30%;
  margin: 20px auto;
  left: 0;
  right: 0;
  /*background-color: rgba(200,0,0,0.5);*/
}

.palabras {
  display: block;
  padding: 20px 0px 5px;
  font-style: italic;
  color: #fff;
  font-size: .9em;
  line-height: 1em;
}

.gestos {
  display: block;
  font-size: .9em;
  line-height: 1em;
}

.efectos {
  display: table-cell;
  vertical-align: middle;
  padding: 40px 40px 35px;
  color: #ddd;
  font-size: .9em;
  line-height: 1.05em;
}

.learnSpell {
  position: absolute;
  top: 0px;
  right: 0px;
  z-index: 5;
  width: 30px;
  height: 30px;
}

</style>
