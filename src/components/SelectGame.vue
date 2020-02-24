<template>
  <div>
    <section>
      <p>Create a Game</p>
      <input v-model="gameName" />
      <button v-on:click="$emit('create-game', gameName)">Create Game</button>
    </section>
    <section>
      <p>Join a Game</p>
      <input v-model="gameId" placeholder="Enter an existing game id" />
      <button v-on:click="$emit('join-game', gameId)">Join</button>
    </section>
  </div>
</template>

<script>
export default {
  props: ["activeGames"],
  data: () => ({
    gameName: "",
    gameId: ""
  })
};
</script>

<style scoped>
div {
  display: flex;
}

p {
  margin: 4px;
}

section {
  padding: 16px;
  background-color: whitesmoke;
  margin: 16px;
}
</style>