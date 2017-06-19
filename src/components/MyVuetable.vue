<template>
  <div class="ui container">

    <div class="vuetable-pagination ui basic segment grid">
      <vuetable-pagination-info ref="paginationInfoTop">
      </vuetable-pagination-info>
      <vuetable-pagination
      ref="paginationTop"
      @vuetable-pagination:change-page="onChangePage"
      >
    </vuetable-pagination>
  </div>
  <vuetable
    ref="vuetable"
    api-url="https://vuetable.ratiw.net/api/users"
    :fields="fields"
    pagination-path=""
    :per-page="20"
    @vuetable:pagination-data="onPaginationData"
  ></vuetable>
  <div class="vuetable-pagination ui basic segment grid">
    <vuetable-pagination-info ref="paginationInfo">
    </vuetable-pagination-info>
    <vuetable-pagination
    ref="pagination"
    @vuetable-pagination:change-page="onChangePage"
    >
  </vuetable-pagination>
</div>
</div>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import accounting from 'accounting'
import moment from 'moment'

export default {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
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
          callback: 'formatNumber',
          visible: false
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
        this.$refs.paginationInfoTop.setPaginationData(paginationData)
        this.$refs.paginationTop.setPaginationData(paginationData)

        this.$refs.paginationInfo.setPaginationData(paginationData)
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
