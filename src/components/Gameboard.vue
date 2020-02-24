<template>
  <div>
    <section>
      <p>Your friend can use the following id to join the game!</p>
      <code>{{ gameId }}</code>
    </section>
    <section v-if="game" class="board">
      <div class="row" :key="row.id" v-for="row in game.board">
        <div
          class="tile"
          :key="tile.pos"
          v-for="tile in row.tiles"
          v-on:click="() => updateTile(row.id, tile.pos)"
        >
          <p>{{ tile.value }}</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { database } from "firebase";

export default {
  props: ["gameId", "user"],
  data: () => ({
    game: null
  }),

methods: {
  updateTile(rowId, pos) {
    const newGame = { ...this.game };

    newGame.board
      .find(row => row.id === rowId)
      .tiles.find(tile => tile.pos === pos).value = this.unit;

    database()
      .ref(`/games/${this.gameId}`)
      .set(newGame);
  }
},

  // Lifecycle hook, runs right after injection.
  created() {
    database()
      .ref(`/games/${this.gameId}`)
      .on("value", snapshot => {
        this.game = snapshot.val();

        if (this.game.creator === this.user.name) {
          this.unit = "x";
        } else {
          this.unit = "o";
        }
      });
  }
};
</script>

<style scoped>
section {
  padding: 16px;
}

.board {
  width: 600px;
  height: 600px;
}

.row {
  display: flex;
}

.tile {
  flex: 1;
  height: 200px;
  border: 1px solid black;
  box-sizing: border-box;
  text-align: center;
}

.tile > p {
  font-size: 60px;
}
</style>
