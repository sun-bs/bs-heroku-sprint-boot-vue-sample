<template>
    <div>
      <template v-if="!this.$store.getters.isAuthenticated">
        <input type="text" v-model="id">
        <input type="password" v-model="password">
        <button @click="login">ログイン</button>
      </template>
      <template v-if="this.$store.getters.isAuthenticated">
        <p>{{this.$store.getters.id}}さん</p>
        <button @click="logout">ログアウト</button>
      </template>
      <!-- ↓追記↓ -->
      <button @click="requestApi">ログインしないと失敗するリクエスト</button>
      <p>{{result}}</p>
      <!-- ↑追記↑ -->
    </div>
</template>


<script>
export default {
  data() {
    return {
      id: '',
      password: '',
      // ↓追記↓
      result: '',
      // ↑追記↑
    };
  },
  methods: {
    login() {
      this.$store.dispatch('login', {
        id: this.id,
        password: this.password
      });
      this.id = '';
      this.password = '';
    },
    logout() {
      this.$store.dispatch('logout');
    },
    // ↓追記↓
    requestApi() {
      this.$axios
        .get('/sample')
        .then(response => {
          this.result = response.data;
        })
        .catch(error => {
          this.result = error;
        })
    }
    // ↑追記↑
  }
};
</script>