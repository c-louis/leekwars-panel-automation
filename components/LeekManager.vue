<template>
  <LeekAction v-if="isLogged"></LeekAction>
  <LeekLogin
    v-else
    v-on:childToParent="onChild"
  ></LeekLogin>
</template>

<script>

import LeekAction from '~/components/LeekAction';
import LeekLogin  from '~/components/LeekLogin';

export default {
  data: () => {
    return {
      isLogged: false,
    };
  },
  methods: {
    async onChild(value) {
      this.isLogged = value;
    }
  },
  components: {
    LeekAction,
    LeekLogin,
  }
}
</script>