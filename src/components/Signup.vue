<template>
  <div class="login">
    <h2>Signup</h2>
    <p>
      Signing up is not required to use the site, but we will be able to save
      your test history so you can see your progress and review qwudstions you
      have missed.
    </p>
    <input v-model="email" type="text" placeholder="Email" />
    <br />
    <input v-model="password" type="password" placeholder="Password" />
    <br />
    <div class="actions">
      <button v-on:click="signup">Signup</button>
      <button @click="close" class="cancel">Cancel</button>
      <a href="#" v-on:click="googs">Login with Google</a>
    </div>
  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";

export default {
  name: "signup",
  data() {
    return {
      email: null,
      password: null
    };
  },
  methods: {
    signup: function() {
      this.$store.dispatch("signup", {
        email: this.email,
        password: this.password
      })
      .then(resp => {
        this.$store.dispatch("toggleSignup");
      });
    },
    close: function() {
      this.$store.dispatch("toggleSignup");
    },
    googs: function() {
      let provider = new firebase.auth.GoogleAuthProvider();
      firebase
        .auth()
        .signInWithPopup(provider)
        .then(result => {
          if (result.user) {
            this.$store.dispatch("showMessage", `Logged in as ${result.user.email}`);
            this.$store.dispatch("toggleSignup");
          }
        })
        .catch((err) => {
          if (err.message) {
            this.$store.dispatch("showMessage", err.message);
          }
        });
    }
  }
};
</script>

<style scoped>
.login {
  display: block;
  width: 400px;
  max-width: 400px;
  z-index: 100;
  background: #fff;
  border-bottom: 1px solid #e5e5e5;
  border-left: 1px solid #e5e5e5;
  border-right: 1px solid #e5e5e5;
  padding: 20px;
}
.actions {
  margin: 10px auto;
}
input {
  margin: 10px auto;
  width: 90%;
  padding: 15px;
  border-radius: 5px;
  font-size: 1.1em;
}
</style>
