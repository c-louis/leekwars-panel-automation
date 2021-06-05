<template>
  <section class="section">
    <LeekManager></LeekManager>
  </section>
</template>

<script>
import Card from '~/components/Card'
import LeekManager from '~/components/LeekManager'

export default {
  name: 'HomePage',

  components: {
    Card,
    LeekManager,
  }
}
</script>
