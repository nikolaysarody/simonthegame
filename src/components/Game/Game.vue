<template>
    <div class="simon">
        <ul>
            <li class="tile red" ref="red" data-tile="1"></li>
            <li class="tile green" ref="green" ></li>
            <li class="tile blue" ref="blue" ></li>
            <li class="tile yellow" ref="yellow" ></li>
        </ul>
    </div>
</template>

<script>
    //import Buttons from "./Buttons";

    export default {
        name: "Game",
        data() {
            return {
                sequence: [],
                copy: [],
                round: 0,
                active: true,
                mode: 'normal',
                colors: ['red','green','blue','yellow'],
                levels: {
                    'easy': 1500,
                    'middle': 1000,
                    'hard': 400,
                }
            }
        },
        props: {
          start: Boolean,
          level: String,
        },
        methods: {
            startGame(){
                this.sequence = [];
                this.copy = [];
                this.round = 0;
                this.active = true;
                this.newRound();
            },
            newRound() {
                this.$emit('new-round', ++this.round);
                this.sequence.push(this.colors[this.randomNumber()-1]);
                this.copy = this.sequence.slice(0);
                this.animate(this.sequence);
            },
            randomNumber() {
                return Math.floor((Math.random()*4)+1);
            },
            animate(sequence) {
                let i = 0;
                let that = this;
                let interval = setInterval(function() {
                    that.playSound(sequence[i]);
                    that.lightUp(sequence[i]);
                    i++;
                    if (i >= sequence.length) {
                        clearInterval(interval);
                        that.activateSimonBoard();
                    }
                }, this.levels[this.level]);
            },
            lightUp(tile) {
                if (this.mode !== 'sound-only') {
                    this.$refs[tile].classList.add('lit');
                    setTimeout(() => this.$refs[tile].classList.remove('lit'), 300);
                }
            },
            playSound(tile) {
                this.$emit('audio-off');
                setTimeout(() => this.$emit('audio', tile), 100);
            },
            activateSimonBoard() {
                let that = this;
                for(let i=0; i<=3; i++){
                    that.$refs[that.colors[i]].onclick = function(e) {
                        that.registerClick(e);
                    };
                    that.$refs[that.colors[i]].onmousedown = function() {
                        that.$refs[that.colors[i]].classList.add('active');
                    };
                    that.$refs[that.colors[i]].onmouseup = function() {
                        that.$refs[that.colors[i]].classList.remove('active');
                    };
                    that.$refs[that.colors[i]].classList.add('hoverable');
                }
            },
            registerClick(e) {
                this.$emit('audio-off');
                this.playSound(e.target.classList[1]);
                let desiredResponse = this.copy.shift();
                let actualResponse = e.target.classList[1];
                this.active = (desiredResponse === actualResponse);
                this.checkLose();
            },
            checkLose() {
                if (this.copy.length === 0 && this.active) {
                    this.deactivateSimonBoard();
                    this.newRound();
                } else if (!this.active) {
                    this.deactivateSimonBoard();
                    this.endGame();
                }
            },
            deactivateSimonBoard() {
                let that = this;
                if (this.mode !== 'free-board') {
                    for(let i=0; i<=3; i++){
                        that.$refs[this.colors[i]].onclick = function() {};
                        that.$refs[that.colors[i]].onmousedown = function() {};
                        that.$refs[that.colors[i]].onmouseup = function() {};
                        that.$refs[that.colors[i]].classList.remove('hoverable');
                    }
                }
            },
            endGame() {
                this.$emit('game-end', this.round);
            },
        },
        watch:{
          start(){
              if(this.start === true){
                  this.startGame();
              }
          }
        },
    }

</script>

<style lang="scss">
    .simon {
        background: white;
        position: relative;
        float: left;
        margin-right: 3em;
        width: 302px;
        height: 295px;
        border-radius: 150px 150px 150px 150px;
        box-shadow: 2px 1px 12px #aaa;
    }
    ul {
        list-style: none;
    }

    ul, li {
        padding: 0;
        margin: 1px;
    }

    .active {
        opacity: 1 !important;
    }

    .clearfix {
        width: 100%;
        clear: both;
    }

    .wrapper {
        width: 540px;
        margin: 0 auto;
    }

    .tile {
        opacity: 0.6;
        -webkit-transition: opacity 250ms ease;
        -moz-transition: opacity 250ms ease;
        -ms-transition: opacity 250ms ease;
        -o-transition: opacity 250ms ease;
        transition: opacity 250ms ease;

        &.lit {
            opacity: 1;
        }
    }

    .hoverable:hover {
        cursor: pointer;
    }

    .red {
        background: #FF5643;
        clip: rect(0px, 300px, 150px, 150px);
        width: 296px;
    }

    .blue {
        background: dodgerblue;
        clip: rect(0px, 150px, 150px, 0px);
        width: 300px;
    }

    .yellow {
        background: #FEEF33;
        clip: rect(150px, 150px, 300px, 0px);
        width: 300px;
    }

    .green {
        background: #BEDE15;
        clip: rect(150px,300px, 300px, 150px);
        width: 296px;
    }

    .red, .blue, .yellow, .green {
        opacity: 0.6;
        height: 290px;
        -webkit-border-radius: 150px 150px 150px 150px;
        border-radius: 150px 150px 150px 150px;
        position: absolute;
        text-indent: 10000px;
    }
    .red:hover, .blue:hover, .yellow:hover, .green:hover {
        border: 2px solid black
    }
</style>