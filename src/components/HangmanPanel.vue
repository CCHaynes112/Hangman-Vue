<template>
    <div class="HangmanCharacter">
      <h1>Category: Movies</h1>
      <img class="HangmanImg" :src="loadImg(failCount)" />
      <h1>{{ word.toUpperCase() }}</h1>
    </div>
</template>

<script>
export default {
    name: 'HangmanPanel',
    props: {
        word: String,
        failCount: Number
    },
    methods: {
        loadImg(failCount) {
            return require(`../assets/img/${ failCount }.png`);
        }
    }
}
</script>

<style scoped>
    .HangmanCharacter {
        background-color: white;
        text-align: center;
        width: 100%;
        letter-spacing: 5px;
        padding: 5px;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
        height: fit-content;
        margin: 10px;
    }
    .HangmanImg {
        width: 50%;
    }
</style>