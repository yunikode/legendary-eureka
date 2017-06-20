<template>
<div class="ui container">

  <filter-bar></filter-bar>
  <vuetable
    ref="vuetable"
    api-url="https://vuetable.ratiw.net/api/users"
    :fields="fields"
    :sort-order="sortOrder"
    pagination-path=""
    :per-page="20"
    :multi-sort="true"
    multi-sort-key="ctrl"
    detail-row-component="my-detail-row"
    :append-params="moreParams"
    @vuetable:pagination-data="onPaginationData"
    @vuetable:cell-clicked="onCellClicked"
  >
    <template slot="actions" scope="props">
      <div class="custom-actions">
        <button
          class="ui icon button"
          @click="onAction('view-item', props.rowData, props.rowIndex)">
          <i class="zoom icon"></i>
        </button>
        <button
          class="ui icon button"
          @click="onAction('edit-item', props.rowData, props.rowIndex)">
          <i class="edit icon"></i>
        </button>
        <button
          class="ui icon button"
          @click="onAction('delete-item', props.rowData, props.rowIndex)">
          <i class="delete icon"></i></button>
      </div>
    </template>
  </vuetable>
  <div class="vuetable-pagination ui basic segment grid">
    <vuetable-pagination-info ref="paginationInfo">
    </vuetable-pagination-info>
    <vuetable-pagination ref="pagination" @vuetable-pagination:change-page="onChangePage">
    </vuetable-pagination>
  </div>
</div>
</template>

<script>
import Vue from 'vue'
import VueEvents from 'vue-events'
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import accounting from 'accounting'
import moment from 'moment'
import DetailRow from './DetailRow'
import FilterBar from './FilterBar'
import FieldDefs from './FieldDefs.js'

Vue.use(VueEvents)

Vue.component('my-detail-row', DetailRow)
Vue.component('filter-bar', FilterBar)

export default {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
  data() {
    return {
      sortOrder: [{
        field: 'email',
        sortField: 'email',
        direction: 'asc'
      }],
      moreParams: {

      },
      fields: FieldDefs
    }
  },
  methods: {
    allcap(val) {
      return val.toUpperCase()
    },
    genderLabel(gen) {
      return gen === 'M' ?
        '<span class="ui teal label"><i class="large man icon"></i>Male</span>' :
        '<span class="ui pink label"><i class="large woman icon"></i>Female</span>'
    },
    formatNumber(num) {
      return accounting.formatNumber(num, 2)
    },
    formatDate(val, fmt = 'D MMM YYYY') {
      return (val == null) ?
        '' :
        moment(val, 'YYYY-MM-DD')
        .format(fmt)
    },
    onPaginationData(paginationData) {
      this.$refs.paginationInfo.setPaginationData(paginationData)
      this.$refs.pagination.setPaginationData(paginationData)
    },
    onChangePage(page) {
      this.$refs.vuetable.changePage(page)
    },
    onAction(action, data, index) {
      console.log('slot action: ' + action, data.name, index)
    },
    onCellClicked (data, field, e) {
      console.log('cellClicked: ', field.name)
      this.$refs.vuetable.toggleDetailRow(data.id)
    },
    onFilterSet (filter) {
      this.moreParams = {
        'filter': filter
      }
      Vue.nextTick( () => this.$refs.vuetable.refresh() )
    },
    onFilterReset () {
      this.moreParams = {}
      Vue.nextTick( () => this.$refs.vuetable.refresh() )
    }
  },
  mounted () {
    this.$events.$on('filter-set', eData => this.onFilterSet(eData))
    this.$events.$on('filter-reset', eData => this.onFilterReset())
  }
}
</script>

<style lang="css">
</style>
