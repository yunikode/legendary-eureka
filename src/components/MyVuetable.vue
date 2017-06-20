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
  name: 'my-vuetable',
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
  props: {
    apiUrl: {
      type: String,
      required: true
    },
    fields: {
      type: Array,
      required: true
    },
    sortOrder: {
      type: Array,
      default() {
        return []
      }
    },
    appendParams: {
      type: Object,
      default() {
        return {}
      }
    },
    detailRowComponent: {
      type: String
    }
  },
  data() {
    return {}
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
      this.appendParams.filter = filterText
      Vue.nextTick( () => this.$refs.vuetable.refresh() )
    },
    onFilterReset () {
      delete this.appendParams.filter
      Vue.nextTick( () => this.$refs.vuetable.refresh() )
    },
    renderVuetable(h) {
      return h (
        'vuetable',
        {
          ref: 'vuetable',
          props: {
            apiUrl: this.apiUrl,
            fields: this.fields,
            paginationPath: "",
            perPage: 10,
            multiSort: true,
            sortOrder: this.sortOrder,
            appendParams: this.appendParams,
            detailRowComponent: this.detailRowComponent,
          },
          on: {
            'vuetable:cell-clicked': this.onCellClicked,
            'vuetable:pagination-data': this.onPaginationData
          },
          scopedSlots: this.$vnode.data.scopedSlots
        }
      )
    },
    renderPagination (h) {
      return h(
        'div',
        { class: {'vuetable-pagination': true, 'ui': true, 'basic': true, 'segment': true, 'grid': true}},
        [
          h('vuetable-pagination-info', { ref: 'paginationInfo'}),
          h('vuetable-pagination', {
            ref: 'pagination',
            on: {
              'vuetable-pagination:change-page': this.onChangePage
            }
          })
        ]
      )
    }
  },
  mounted () {
    this.$events.$on('filter-set', eData => this.onFilterSet(eData))
    this.$events.$on('filter-reset', eData => this.onFilterReset())
  },
  render (h) {
    return h (
      'div',
      {
        class: { ui: true, container: true}
      },
      [
        h('filter-bar'),
        this.renderVuetable(h),
        this.renderPagination(h)
      ]
    )
  }
}
</script>

<style lang="css">
</style>
