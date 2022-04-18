<template>
  <div>
    <h2>{{title}}</h2>
    <h3>갱신된 날짜 : {{updateDate}}</h3>
    <section class="gridTableWrap">
      <vuetable ref="vuetable"
        :api-mode="false"
        :fields="fields"
        :per-page="perPage"
        :data-manager="dataManager"
        :css="css.table"
        @vuetable:row-clicked="onRowClicked"
        :row-class="onRowClass"
        pagination-path="pagination"
        @vuetable:pagination-data="onPaginationData"
      ></vuetable>
      <div class="vuetable-pagination tableBottom">
        <vuetable-pagination ref="pagination"
          :css="css.pagination"
          @vuetable-pagination:change-page="onChangePage"
        ></vuetable-pagination>
      </div>
    </section>
  </div>
</template>

<script>
import Vuetable from 'vuetable-2'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import _ from 'lodash'
import VuetableCss from '../../assets/vuetable'
export default {
  components: {
    Vuetable,
    VuetablePagination
  },
  data () {
    return {
      css: VuetableCss,
      fields: [
        {
          name: 'id',
          title: '<i class="grey user outline icon"></i>분류',
          width: '10%'
        },
        {
          name: 'name',
          title: '<i class="grey mail outline icon"></i>이름',
          width: '15%'
        },
        {
          name: 'price',
          title: '<i class="grey birthday icon"></i>가격',
          width: '10%'
        },
        {
          name: 'value',
          title: '<i class="grey birthday icon"></i>개수',
          width: '15%'
        },
        {
          name: 'original',
          title: '<i class="grey birthday icon"></i>원자재',
          width: '20%'
        }
      ],
      perPage: 0,
      data: [],
      titie: '시세표',
      updateDate: ''
    }
  },
  created () {
    var today = new Date()
    var year = today.getFullYear()
    var month = ('0' + (today.getMonth() + 1)).slice(-2)
    var day = ('0' + today.getDate()).slice(-2)

    var dateString = year + '-' + month + '-' + day
    this.updateDate = dateString
  },
  methods: {
    dataManager (sortOrder, pagination) {
      let local = this.data
      // sortOrder can be empty, so we have to check for that as well
      if (sortOrder.length > 0) {
        // console.log('orderBy:', sortOrder[0].sortField, sortOrder[0].direction)
        local = _.orderBy(
          local,
          sortOrder[0].sortField,
          sortOrder[0].direction
        )
      }

      pagination = this.$refs.vuetable.makePagination(
        local.length,
        this.perPage
      )
      let from = pagination.from - 1
      let to = from + this.perPage

      return {
        pagination: pagination,
        data: _.slice(local, from, to)
      }
    },
    onRowClicked (dataItem, event) {
      console.log(dataItem)
      // this.$emit('show-data', dataItem)
    },
    onRowClass (dataItem) {
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

<style>
</style>
