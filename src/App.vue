<template>
  <section>
    <h1>Tic Tac Toe</h1>
    <Login v-if="!user" v-on:login="addUser" />
    <SelectGame v-if="user" v-on:create-game="createGame" v-on:join-game="joinGame" />
    <hr />
    <Gameboard :user="user" v-if="activeGame" :gameId="activeGame" />
  </section>
</template>

<script>
import { database } from "firebase";
import uuid from "uuid";
import board from "./board";
import Gameboard from "./components/Gameboard";
import Login from "./components/Login";
import SelectGame from "./components/SelectGame";

export default {
  name: "app",
  components: { Gameboard, Login, SelectGame },
  data: () => ({
    user: null,
    allUsers: [],
    activeGame: null,
  }),
  methods: {

addUser(name) {
  const userObject = this.allUsers.find(u => u.name === name);

  if (userObject) {
    this.user = userObject;
    return;
  }

  const newUser = { name, games: [] };

  database()
    .ref(`users`)
    .set([...this.allUsers, newUser]);

  this.user = newUser;
},

    createGame(name) {
      const id = uuid();
      const newGame = { creator: this.user.name, name, id, board };
      database()
        .ref(`/games/${id}`)
        .set(newGame);
      this.activeGame = id;
    },
    joinGame(id) {
      this.activeGame = id;
    }
  },
  created() {
    database()
      .ref("users")
      .on("value", snapshot => {
        if (snapshot.val()) {
          this.allUsers = snapshot.val();
        }
      });
  }
};
</script>
