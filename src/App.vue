<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link v-if="!isLoggedIn" to="/login">Login</router-link>
    <span v-else @click="logout">Logout</span>
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
</style>
