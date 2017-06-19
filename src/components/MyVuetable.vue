<template lang="html">
  <vuetable
    ref="vuetable"
    api-url="https://vuetable.ratiw.net/api/users"
    :fields="fields"
  ></vuetable>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
export default {
  components: {
    Vuetable
  },
  data () {
    return {
      fields: [
        {
          name: 'name'
        },
        {
          name: 'email'
        },
        {
          name: 'birthdate',
          titleClasss: 'center aligned',
          dataClass: 'center aligned'
        },
        {
          name: 'nickname'
        },
        {
          name: 'gender',
          titleClasss: 'center aligned',
          dataClass: 'center aligned'
        },
        {
          name: 'salary',
          titleClasss: 'center aligned',
          dataClass: 'right aligned'
        }
      ]
    }
  }
}
</script>

<style lang="css">
</style>
