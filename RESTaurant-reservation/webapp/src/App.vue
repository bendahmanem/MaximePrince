<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/login">Login</router-link> |
    <router-link to="/signup">Signup</router-link> |
    <router-link to="/explore" v-show="true">Explore</router-link>
    |
    <a  href="#" v-on:click="logout">Logout</a>
  </div>
  <router-view />

  <div class="footer">
    <div class="c">
      <p>RESTaurant Reservation</p>
    </div>
    <a
      href="https://github.com/AlessioLuciani/RESTaurant-reservation"
      target="_blank"
    >
      <img class="b" src="./assets/github-icon.png" />
    </a>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import router from './router';
import Utils from './utils';

@Options({
  components: {},
})
/* eslint class-methods-use-this: ["error", { "exceptMethods": ["mounted", "logout"] }] */
export default class App extends Vue {
  mounted() {
    if (!Utils.isLogged) {
      if (localStorage.authType === '0') {
        Utils.loginUser(
          localStorage.authEmail,
          localStorage.authToken,
          (response) => {
            Utils.isLogged = response.error === undefined;
          },
        );
      } else {
        Utils.loginRestaurant(
          localStorage.authEmail,
          localStorage.authToken,
          (response) => {
            Utils.isLogged = response.error === undefined;
          },
        );
      }
    } else {
      console.log('[Cached] user is currently logged');
    }
    // Utils.signInSilently();
  }

  logout() {
    console.log('Removing invalid credentials from local storage');
    Utils.resetUserCredentials();
    this.$router.go(0);
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
  background-color: whitesmoke;
  color: #2c3e50;
  position: relative;
  min-height: 100vh;
}

#nav {
  padding: 30px;
  text-align: right;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 70px;
  background-color: #24292e;
  color: white;
  text-align: center;
}

.a {
  position: relative;
  width: 200px;
  height: 200px;
}
.b {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 20px;
  margin: auto;
  height: 32px;
}
.c {
  position: absolute;
  top: 5px;
  bottom: 0;
  left: 20px;
  margin: auto;
  height: 32px;
}

.main {
  max-width: 1000px;
  height: 800px;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  max-height: 100%;
  overflow: auto;
}
</style>
