<template>
<div class="container">
  <div class="columns">
    <div class="column is-full">
      <div class="card">
        <header class="card-header">
          <p class="card-header-title has-text-grey is-centered">
            Manage your LeekWars
          </p>
        </header>
        <div class="card-content">
          <div class="content has-text-centered">
            <div v-if="farmer != null">
              <LeekList
                v-bind:farmer="farmer"
                v-on:childToParent="onSelectedLeek"
              ></LeekList>
            </div>
            <b-button v-else @click="load">Load</b-button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-if="farmer != null">
    <LeekManual
      v-bind:farmer="farmer"
      v-bind:leekapi="leekapi"
      ></LeekManual>
  </div>
</div>
</template>

<script>
export default {
  created() {
    this.load();
  },
  data: () => {
    return {
      farmer: null,
      selectedLeeks: [],
      leekapi: require("@c-louis/leekwars-api-automation"),
    };
  },
  methods: {
    async load() {
      let data = this.leekapi.loginWithToken().then((res) => {
          this.farmer = res.farmer;
          this.loadFarmerOpponent();
        this.loadLeeksOpponent();
      }).catch((err) => {
          console.log(err);
      });
    },
    onSelectedLeek(selectedLeeks) {
      this.selectedLeeks = selectedLeeks;
      console.log(this.selectedLeeks);
    },
  }
}
</script>
