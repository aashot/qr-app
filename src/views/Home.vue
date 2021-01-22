<template>
  <div class="home">
    <b-card no-body>
      <b-tabs 
        pills 
        justified
        class="tabs-nav"
      >
        <Scan />
        <Create />
      </b-tabs>
    </b-card>
  </div>
</template>

<script>
import Scan from '@/components/Scan'
import Create from '@/components/Create'

export default {
  name: 'Home',
  components: {
    Scan,
    Create
  }
}
</script>

<style lang="scss">
  .tabs-nav {
    a {
      border-radius: 0 !important;
      transition: 0.2s ease-in-out;
    }
  }
</style>
