<template>
    <div class="options">
        <h2>Настройки игры:</h2>
            <input type="radio" id="easy" name="level" value="easy" checked @click="$emit('level', 'easy')"><label for="easy">Легкий</label><br>
            <input type="radio" id="middle" name="level" value="middle" @click="$emit('level', 'middle')"><label for="middle">Средний</label><br>
            <input type="radio" id="hard" name="level" value="hard" @click="$emit('level', 'hard')"><label for="hard">Тяжелый</label><br>
    </div>
</template>

<script>
    export default {
        name: "Options"
    }
</script>

<style scoped lang="scss">
    .options {
        margin-right: -10%;

        h2 {
            margin-top: 30px;
            margin-bottom: 0;
        }

        input[type="radio"] {
            margin-right: 10px;

            + label {
                font-size: 18px;
            }
        }


    }
</style>