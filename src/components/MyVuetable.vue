<template>
  <div class="ui container">

  <vuetable
    ref="vuetable"
    api-url="https://vuetable.ratiw.net/api/users"
    :fields="fields"
    pagination-path=""
    @vuetable:pagination-data="onPaginationData"
  ></vuetable>
  <vuetable-pagination
    ref="pagination"
    @vuetable-pagination:change-page="onChangePage"
    >
  </vuetable-pagination>
</div>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePaginationDropdown'
import accounting from 'accounting'
import moment from 'moment'

export default {
  components: {
    Vuetable,
    VuetablePagination
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
      },
      onPaginationData (paginationData) {
        this.$refs.pagination.setPaginationData(paginationData)
      },
      onChangePage (page) {
        this.$refs.vuetable.changePage(page)
      }
    }
}
</script>

<style lang="css">
</style>
