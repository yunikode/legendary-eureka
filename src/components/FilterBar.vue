<template lang="html">
  <div class="filter-bar ui basic segment grid">
    <div class="ui form">
      <div class="inline field">
        <label>Search for:</label>
        <input type="text" v-model="filterText" class="three wide column">
        <button class="ui primary button" @click="doFilter">Go</button>
        <button class="ui button" @click="resetFilter">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      filterText: ''
    }
  },
  methods: {
    doFilter () {
      console.log('doFilter: ', this.filterText)
    },
    resetFilter () {
      this.filterText = ''
      console.log('resetFilter')
    }
  }
}
</script>

<style lang="css">
</style>
