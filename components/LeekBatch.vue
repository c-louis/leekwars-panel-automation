<template>
  <div class="content">
    <p class="title is-3 has-text-centered">Batch Fight</p>
    <div class="columns" v-if="farmer != null">
      <div class="column is-full">
        <div class="card">
          <div class="card-content">
            <div class="content has-text-centered">
                <b-field label="Type" position="is-centered">
                  <b-radio-button v-model="batchSelector"
                    native-value="leek"
                    type="is-danger"
                    :disabled="selectedLeeks.length == 0">
                    Leek
                  </b-radio-button>
                  <b-radio-button v-model="batchSelector"
                    native-value="farmer"
                    type="is-success">
                    Farmer
                  </b-radio-button>
                </b-field>
              <br />
              <b-field label="Enable fast mode">
                <b-checkbox v-model="fastMode" type="is-danger">
                </b-checkbox>
              </b-field>
              <b-field label="Batch Size">
                <b-slider size="is-large" v-model="batchSize" :tooltip-type="sliderType(batchSize)" :max="farmer.fights" type="is-danger" rounded></b-slider>
              </b-field>
              <b-button @click="batchFight" type="is-primary is-light">Fight</b-button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    farmer: {
      type: Object,
      required: true
    },
    leekapi: {
      type: Object,
      required: true
    },
    selectedLeeks: {
      required: true,
    }
  },
  data: () => {
    return {
      batchSelector: 'farmer',
      batchSize: 0,
      fastMode: false,
      fightDone: 0
    }
  },
  methods: {
    sliderType(size) {
      if (size > (this.farmer.fights/4) && size < ((this.farmer.fights/4) * 3)) {
        return "is-warning";
      } else if (size >= ((this.farmer.fights/4) * 3)) {
        return "is-success";
      }
      return "is-danger";
    },
    batchFight() {
      if (this.batchSize == 0) {
        return;
      }
      if (this.batchSelector == 'farmer') {
        this.farmerBatchFights();
      } else {
        this.leekBatchFights();
      }
    },
    farmerBatchFights() {
        this.leekapi.farmerBatchFights(
          this.batchSize,
          true,
          this.fastMode,
          this.showFightResult
        ).then((res) => {
        }).catch((err) => {
          console.log(err);
        });
    },
    leekBatchFights() {
      for (var leekid of this.selectedLeeks) {
        this.leekapi.leekBatchFights(
          leekid,
          this.batchSize / this.selectedLeeks.length,
          true,
          this.fastMode,
          this.showFightResult
        ).then((res) => {
        }).catch((err) => {
          console.log(err);
        });
      }
    },
    showFightResult(fight) {
      console.log(fight);
      this.$buefy.notification.open({
        duration: 2500,
        message: `Fight : ${fight.id} is over !`,
        position: 'is-bottom-right',
        type: fight.winner == 2 ? 'is-danger' : 'is-success',
        hasIcon: true,
        queue: false,
      });
    },
  }
}
</script>