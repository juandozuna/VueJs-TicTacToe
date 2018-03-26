<template>
  <div id="app">
      <br>  <br>

      <board></board>

  </div>
</template>

<script>
/*eslint-disable*/
import board from './components/board.vue'

export default {
  name: 'App',
  components: {
      board
    },
    data(){
        return {};
    }
}
</script>

<style>

</style>
