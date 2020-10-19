<template>
    <div class="info">
        <h2>
            Раунд:
            <span>{{round}}</span>
        </h2>
        <button v-on:click="$emit('isGameStarted')">Старт</button>
        <p v-if="lose">Вы проиграли после {{loseround}} раунда!</p>
    </div>
</template>

<script>
    export default {
        name: "Info",
        props: {
            round: Number,
            lose: Boolean,
            loseround: Number,
        },
    }
</script>

<style scoped lang="scss">
    .info {
        button{
            width: 5em;
            box-sizing: border-box;
            font-size: 1.4em;
            -webkit-border-radius: 10px 10px 10px 10px;
            border-radius: 10px 10px 10px 10px;
            background: #6DABE8;
            border: none;
            padding: 0.3em 0.6em;

            &:hover {
                background: #78BCFF;
            }
        }
    }
</style>