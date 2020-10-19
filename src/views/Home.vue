<template>
    <div class="container">
        <h1>Simon The Game</h1>
        <game v-bind:start="gameStart"
              v-bind:level="level"
              v-on:game-end="gameEnd"
              v-on:new-round="newRound"
              v-on:audio-off="audioOff"
              v-on:audio="audio"
        ></game>
        <info v-bind:lose="lose"
              v-bind:round="round"
              v-bind:loseround="loseround"
              v-on:isGameStarted="gameStarted()"
        ></info>
        <options v-on:level="difficultyLevel"
        ></options>
        <div ref="audio"></div>
        <audio autoplay v-if="isAudioOn">
            <source :src="audioOgg" type="audio/ogg"/>
            <source :src="audioMp3" type="audio/mp3"/>
        </audio>
    </div>
</template>

<script>

    import Game from '@/components/Game/Game'
    import Options from "../components/Game/Options";
    import Info from "../components/Game/Info";

    export default {
        name: 'Home',
        components: {
            Info,
            Options,
            Game
        },
        data() {
            return {
                lose: false,
                gameStart: false,
                isAudioOn: false,
                tileOgg: '',
                tileMp3: '',
                round: 0,
                loseround: 0,
                tile: '',
                level: 'easy',
            }
        },
        computed: {
            audioOgg() {
                return require(`../assets/sounds/${this.tileOgg}`);
            },
            audioMp3() {
                return require(`../assets/sounds/${this.tileMp3}`);
            }
        },
        methods: {
            difficultyLevel(e){
                this.level = e;
            },
            gameEnd(e) {
                this.gameStart = false;
                this.lose = true;
                this.round = 0;
                this.loseround = e;
            },
            gameStarted() {
                this.lose = false;
                this.gameStart = true;
                setTimeout(() => this.gameStart = false, 100);
            },
            newRound(e) {
                this.round = e;
            },
            audioOff() {
                this.isAudioOn = false;
            },
            audio(e) {
                this.isAudioOn = true;
                this.tile = e;
                this.tileOgg = e + '.ogg';
                this.tileMp3 = e + '.mp3';
            }
        }
    }
</script>

<style scoped>
    .container {
        width: 540px;
        margin: 0 auto;
    }

    h1 {
        margin: 1em 0 2em;
        text-align: center;
    }
</style>