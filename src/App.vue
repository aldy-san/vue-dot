<template>
  <div id="nav">
    <router-link v-if="!isLoggedIn" to="/login">LOGIN</router-link>
    <template v-else>
      <router-link to="/">HOME</router-link> |
      <span @click="logout">LOGOUT</span>
    </template>
  </div>
  <router-view />
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false,
    };
  },
  methods: {
    logout() {
      localStorage.removeItem("user");
      this.$router.push("/login");
    },
  },
  watch: {
    $route() {
      this.isLoggedIn = !!localStorage.getItem("user");
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
span {
  text-decoration: underline;
  cursor: pointer;
}

.btn {
  border-radius: 10px;
  padding: 8px;
  /*width: 100%;*/
  background: #000;
  color: white;
  cursor: pointer;
}

.btn:hover {
  background: #414141;
}
</style>
