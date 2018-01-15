<template>
  <nav id='navbar'>
    <ul>
      <li :class="{ active: isActive('all') }"><a v-on:click="setMenuOption" title="all">Todos los conjuros</a></li>
      <li :class="{ active: isActive('pj') }"><a v-on:click="setMenuOption" title="pj">{{namedCharacter.name}}</a></li>
      <li :class="{ active: isActive('lvlup') }"><a v-on:click="setMenuOption" title="lvlup">Comprar conjuros</a></li>
      <li><a>SP: {{ this.namedCharacter.points }}</a></li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'navbar',
  props: ['namedCharacter'],
  methods: {
    setMenuOption: function (event) {
      this.$emit('setMenuOption', event.currentTarget.title)
    },
    isActive: function (opt) {
      if (this.namedCharacter.selectedMenuOption === opt) return true
      return false
    }
  },
  data () {
    return { }
  }
}

</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
nav {
  padding-top: 130px;
}
ul {
  list-style-type: none;
  display: flex;
  flex-flow: wrap;
  justify-content: center;
  margin: 0 -5px;
  padding: 10px;
  position: fixed;
  top: 5px;
  z-index: 10;
}

a {
  position: relative;
  text-decoration: none;
  cursor: default;
  display: block;
  padding: 15px;
  min-width: 60px;
  background-color: #282C34;
  color: #fff;
  border: thin solid #fff;
}
a::after {
  content: "";
  position: absolute;
  left: 50%;
  height: 3px;
  width: 0;
  bottom: 1px;
  background: #ffffff;
}

li:hover a::after {
  transition: all 0.2s;
  left: 0;
  width: 100%;
}

li.active a::after {
  transition: all 0.2s;
  left: 0;
  width: 100%;
  height: 4px;
}

@media only screen and (min-device-width: 768px) and (max-device-width: 1024px) {
  li:hover a::after {
    content: none;
  }
}
</style>
