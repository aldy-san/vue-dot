<template>
  <div class="container">
    <h1>Login</h1>
    <div class="form-group">
      <label for="username">Username</label>
      <input
        v-model="form.username"
        id="username"
        type="text"
        class="form-input"
      />
    </div>
    <div class="form-group">
      <label for="password">Password</label>
      <input
        v-model="form.password"
        id="password"
        type="password"
        class="form-input"
      />
    </div>
    <button class="btn" @click="login">Login</button>
    <span>{{ errorMessage }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      errorMessage: "",
      form: {
        username: "",
        password: "",
      },
      // real practice with api
      credentials: {
        username: "admin",
        password: "admin123",
      },
    };
  },
  methods: {
    login() {
      if (
        this.form.username === this.credentials.username &&
        this.form.password === this.credentials.password
      ) {
        localStorage.setItem("user", this.form.username);
        this.$router.push("/");
      } else {
        this.errorMessage = "Kredensial yang anda masukkan salah!";
      }
    },
  },
  mounted() {
    if (localStorage.getItem("user")) {
      this.$router.push("/");
    }
  },
};
</script>

<style scoped>
.container {
  margin: 0 auto;
  max-width: 220px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.form-group {
  display: flex;
  flex-direction: column;
  /*align-items: flex-start;*/
  gap: 6px;
  width: 100%;
}
.form-group input {
  display: flex;
  border: 1px solid grey;
  border-radius: 10px;
  padding: 8px;
}
.btn {
  border-radius: 10px;
  padding: 8px;
  width: 100%;
  background: #000;
  color: white;
}
.btn:hover {
  background: #414141;
}
span {
  color: red;
}
</style>
