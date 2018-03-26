
<template>
    <div >
        <div class="game">
           <div class="gameTitle">
               <h1>{{gameInformation}}</h1>
           </div>
           <div class="board">
               <div>
                   <square v-for="(item,i) in squares" :key="i"
                            @mouseover="item.passed(player)"
                            @click="clickedTile(i)"
                            :played="item.played"
                            :player="item.player"
                            :highligthed="item.highligthed"
                            @leave="item.leave()"
                   >{{item.inside}}</square>
               </div>
           </div>
        </div>
        <button type="button" name="button" @click="reset">Reset Game</button>
    </div>
</template>

<script>
import square from './Square.vue'
/*eslint-disable*/

class Square {
    constructor(){
        this.inside = ''; //Can be either X or O;
        this.played = false;
        this.player = '';
        this.highligthed = false;
    }


    getInside(){
        return this.inside;
    }

    passed(player){
        if(!this.played){
            this.inside = player;
        }
    }

    clicked(player){
          if(!this.played){
            this.played = true;
            this.player = player;
        }else {
            alert('Esta casilla ya fue jugada');
        }
    }
    isPlayed(){
        return !this.played;
    }
    highlight(){
        this.highligthed = true;
    }
    leave(){
        if(!this.played)
            this.inside = '';
    }
    reset(){
        this.inside = ''; //Can be either X or O;
        this.played = false;
        this.player = '';
        this.highligthed = false;
    }

}


export default {
    name: "board",
    components: {
      square
    },
    data(){
        return {
            squares: new Array(9).fill().map( s => new Square()),
            player: 'X',
            moves: 0,
            gameActive: true,
            winner: ''
        };
    },
    computed:{
        gameInformation(){
            let t = '';
            if(this.gameActive){
                 return "It's " + this.player + "'s turn";
            }else{
                if(this.winner === 'Tie')
                   return "Game Over | It's a tie";
                else {
                   return `${this.winner}'s is the Winner`;
                }
            }
        }
    },
    methods: {
        clickedTile(i){
            if(this.gameActive){
                let p = this.player;
                console.log(`Clicked Tile ${i}`);

                if(!this.squares[i].played)
                    this.changePlayer();

                this.squares[i].clicked(p);
            }
        },
        changePlayer(){
            this.checkWinner();
            if(this.player === 'O')
                this.player = 'X';
            else {
                this.player = 'O';
            }

            this.moves++;

        },
        checkWinner(){
            const winningCombinations = [
                [0,1,2], [3,4,5],[6,7,8],
                [0,3,6], [1,4,7],[2,5,8],
                [0,4,8], [2,4,6]
            ];
            winningCombinations.forEach((wc) => {
                const [a,b,c] = wc;
                const sqA = this.squares[a];
                const sqB = this.squares[b];
                const sqC = this.squares[c];

                if(sqA.inside && sqA.inside === sqB.inside && sqA.inside === sqC.inside){
                    this.gameActive = false;
                    this.winner = sqA.player;

                    sqA.highligthed = sqB.highligthed = sqC.highligthed = true;
                }

            });

            if(this.moves >= this.squares.length){
                this.winner = "Tie";
                this.gameActive = false;

            }
        },
        reset(){
            this.squares = new Array(9).fill().map( s => new Square());
            this.player = 'X';
            this.moves = 0;
            this.gameActive = true;


        }
    }
}
</script>

<style lang="css">
    .game{
        margin: 0 auto;
        width: 600px;
        border-radius: 6px 6px 0px 0px ;
    }

    .gameTitle {
        overflow: hidden;
        background-color: #eee;

        height: 64px;

        border: black solid 2px;
        border-radius: 6px 6px 0px 0px;
        border-bottom: DeepSkyBlue 2px solid;


    }

    div.game div.gameTitle h1 {
        font-family: sans-serif;
        font-size: 2em;
        vertical-align: middle;
        text-align: center;
        text-overflow: clip;
        position: relative;
        top: -11%;
        color: #334455 ;
        /*top: 10%;*/

    }


    .board {
        height: 500px;
        border: black solid 2px;
        border-radius: 0px 0px 6px 6px;
        border-top: none;
        font-family: cursive;
        background: repeating-linear-gradient(
              -55deg,
              #96d1ff,
              #96d1ff 10px,
              #a3d7ff 10px,
              #a3d7ff 20px
            );
        display: flex;
        flex-wrap: wrap;
        padding: 5px;
    }

    div.board div {
        margin: 0 auto;

        width: inherit - 10px;
        display: flex;
        flex-wrap: wrap;
    }

    button {
        margin: 0 auto;
        position: relative;
        left: 46.5%;
        margin-top: 10px;
        font-family:sans-serif;
        font-size: 1.25em;
        border-radius: 5px;
        border:lightGray solid 2px;
        background-color: dodgerBlue;
        color: white;
    }
</style>
