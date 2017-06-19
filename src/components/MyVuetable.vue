<template lang="html">
  <vuetable
    ref="vuetable"
    api-url="https://vuetable.ratiw.net/api/users"
    :fields="fields"
  ></vuetable>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import accounting from 'accounting'
import moment from 'moment'

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
          dataClass: 'center aligned',
          callback: 'formatDate|DD-MM-YYYY'
        },
        {
          name: 'nickname',
          callback: 'allcap'
        },
        {
          name: 'gender',
          titleClasss: 'center aligned',
          dataClass: 'center aligned',
          callback: 'genderLabel'
        },
        {
          name: 'salary',
          titleClasss: 'center aligned',
          dataClass: 'right aligned',
          callback: 'formatNumber'
        }
      ]
    }
  },
  methods: {
    allcap (val) {
      return val.toUpperCase()
    },
    genderLabel (gen) {
      return gen === 'M'
        ? '<span class="ui teal label"><i class="large man icon"></i>Male</span>'
        : '<span class="ui pink label"><i class="large woman icon"></i>Female</span>'
    },
    formatNumber (num) {
      return accounting.formatNumber(num, 2)
    },
    formatDate (val, fmt = 'D MMM YYYY') {
      return (val == null)
        ? ''
        : moment(val, 'YYYY-MM-DD').format(fmt)
    }
  }
}
</script>

<style lang="css">
</style>
