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
    <p class="title is-3 has-text-centered">Manual Fight</p>
    <div class="columns" v-if="farmer != null">
    <div class="column is-half">
      <div class="card">
          <div class="header">
            <p class="card-header-title has-text-grey is-centered">
              Leeks
            </p>
          </div>
          <div class="card-content">
            <div class="content has-text-centered">
              <table>
                  <thead>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Talent</th>
                    <th></th>
                  </thead>
                  <tbody v-for="(opponents, leekid) in leeksOpponent" :key="leekid">
                      <tr>
                        <th colspan="4">{{ Object.values(farmer.leeks).find(element => element.id == leekid).name }}</th>
                      </tr>
                      <tr v-for="opponent of opponents" :key="opponent.id">
                        <td>{{ opponent.id }}</td>
                        <td>{{ opponent.name }}</td>
                        <td>{{ opponent.talent }}</td>
                        <td>
                          <b-button @click="leekFight(leekid, opponent.id)">Fight</b-button>
                        </td>
                      </tr>
                  </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-half">
        <div class="card">
          <div class="header">
            <p class="card-header-title has-text-grey is-centered">
            Farmers
            </p>
          </div>
          <div class="card-content">
            <div class="content has-text-centered">
              <table>
                <thead>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Talent</th>
                  <th></th>
                </thead>
                <tbody>
                  <tr v-for="opponent of farmerOpponents" :key="opponent.id">
                    <td>{{ opponent.id }}</td>
                    <td>{{ opponent.name }}</td>
                    <td>{{ opponent.talent }}</td>
                    <td>
                      <b-button @click="farmerFight(opponent.id)">Fight</b-button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data: () => {
    return {
      farmer: null,
      selectedLeeks: [],
      leeksOpponent: {},
      farmerOpponents: [],
      leekapi: require("@c-louis/leekwars-api-automation"),
    };
  },
  methods: {
    async loadLeeksOpponent() {
      var leeks = Object.values(this.farmer.leeks);
      this.leeksOpponent = {};
      for (var leek of leeks) {
        let data = this.leekapi.leekOpponents(leek.id).then((res) => {
          this.$set(this.leeksOpponent, leek.id, res['opponents']);
        }).catch((err) => {
          console.log(err);
        });
        await data;
      }
    },
    loadFarmerOpponent() {
      this.leekapi.farmerOpponents().then((res) => {
        this.farmerOpponents = res['opponents'];
      }).catch((err) => {
        console.log(err);
      });
    },
    async load() {
      let data = this.leekapi.loginWithToken().then((res) => {
          this.farmer = res.farmer;
          this.loadFarmerOpponent();
        this.loadLeeksOpponent();
      }).catch((err) => {
          console.log(err);
      });
    },
    farmerFight(target_id) {
      this.leekapi.farmerFight(target_id, true).then((res) => {
        console.log(res);
        this.loadFarmerOpponent();
      }).catch((err) => {
        console.log(err);
      });
    },
    leekFight(leek_id, target_id) {
      this.leekapi.leekFight(leek_id, target_id, true).then((res) => {
        console.log(res);
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
