<template>
    <div id="ModalBackground">
        <div id="Modal">
            <h1 v-if="won">You win!</h1>
            <h1 v-else>You lost :(</h1>
            <h2>The movie was {{ currentWord }}</h2>
            <button v-on:click="reloadPage">Play again</button>
        </div>
    </div>
</template>

<script>

export default {
  name: 'Modal',
  props: {
      won: Boolean,
      currentWord: String,
  },
  methods: {
      reloadPage() {
          window.location.reload();
      }
  }
}
</script>

<style scoped>
#Modal {
    position: fixed;
    background-color: white;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    top: 50%;
    left: 50%;
    right: auto;
    bottom: auto;
    margin-right: -50%;
    padding: 30px;
    transform: translate(-50%, -50%);
    text-align: center;
}
#ModalBackground {
    position: fixed;
    background-color: rgba(25, 25, 25, 0.6);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}
</style>