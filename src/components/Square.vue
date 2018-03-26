<template lang="html">
  <div class="container" >
      <div class="square" @mouseover="mouse"  @click="clicked" :class="playedClass" @mouseleave="leave">
          <h1><slot></slot></h1>
      </div>
  </div>
</template>

<script>
export default {
    name: 'square',
    props:['played', 'player', 'highligthed'],
    data(){
        return{
            play: this.played,
            turned: this.player
        };
    },
    computed: {
        playedClass(){
            let t = ''
            if(this.played){
                if(this.player === 'X')
                   t = 'playerX ';
                else if(this.player === 'O')
                   t = 'playerO ';
                else {
                    t = '';
                }

                if(this.highligthed)
                    t += 'highlight';
            }
            return t;


        }
    },
    methods: {
      mouse(){
          this.$emit('mouseover');
      },
      clicked(){
          console.log('clicked');
          this.$emit('click');

      },
      leave(){
          this.$emit('leave');

      }
    }
}
</script>

<style lang="css">
    div.container{
        width: 30%;
        height: 30%;
        background-color: rgba(100,100,100,0.4);
        border: solid black 1px;
        border-radius: 5px;
        padding: 5px;
        display: block;
    }

    div.container div.square{
        width: 100px;
        height: 100px;
        margin: auto;
        position: relative;
        background-color: rgba(255,255, 255,0.8);
        border-radius: 2px;
        box-shadow:  0px 0px 15px 10px rgba(255,255, 255,0.8);
        opacity: 0;
         color: rgba(0,0,0, 0.3);
        transition: opacity 0.5s;
    }
    div.container div.square:hover{
        opacity: 1;
        color: rgba(0,0,0, 0.3);
        transition: opacity 0.5s;
    }

    div.container div.square h1{
        font-size: 4.9em;
        text-align: center;
        vertical-align: middle;
        position: relative;
        margin: auto;
        cursor:default;
    }

    div.container div.square.playerO {
        background-color: #ea87ff;
        width: 300px;
        height: 150px;
        box-shadow:  0px 0px 15px 10px rgba(255,255, 255,0.0);
        border-radius: 5px;
        color: rgba(0,0,0, 0.6);
        opacity: 1;
        transition: opacity 0.5s, color 1s, width 0.7s, height 0.5s, box-shadow 0.3s, background-color 1.2s;


    }

    div.container div.square.playerX {
        background-color: #ffd26b;
        width: 300px;
        height: 150px;
        box-shadow:  0px 0px 15px 10px rgba(255,255, 255,0.0);
        border-radius: 5px;
        opacity: 1;
        color: rgba(0,0,0, 0.6);
        transition: opacity 0.5s, color 1s, width 0.7s, height 0.5s, box-shadow 0.3s, background-color 1.2s;


    }

    div.container div.square.playerO.highlight, div.container div.square.playerX.highlight, .highlig{
        background-color: #8cf442;
        color: red;
        transition: background-color ease 2.5s, color 3.2s;
    }


</style>
