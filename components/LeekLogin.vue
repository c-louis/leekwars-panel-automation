<template>
  <div class="column is-full">
    <div class="card">
      <header class="card-header">
        <p class="card-header-title has-text-grey is-centered">
          Connect to LeekWars
        </p>
      </header>
      <div class="card-content">
        <div class="content has-text-centered">
          <b-field label="Login">
              <b-input v-model="login" placeholder="Login" rounded></b-input>
          </b-field>
          <b-field label="Password">
            <b-input type="password"
                placeholder="Password"
                v-model="password"
                password-reveal rounded>
            </b-input>
        </b-field>
        <b-button type="is-primary" @click="connect">Connect</b-button>
        <p> {{ error }} </p>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isLogin: Boolean,
  },
  data: () => {
    return {
      login: '',
      password: '',
      error: '',
    };
  },
  methods: {
    async connect() {
      const leekapi = require("@c-louis/leekwars-api-automation");
      leekapi.login(this.login, this.password).then((res) => {
          if (res == true) {
            leekapi.storeToken();
            this.$emit('childToParent', true);
          } else {
            
          }
      }).catch((err) => {
          this.error = err;
      });
    }
  }
}
</script>
