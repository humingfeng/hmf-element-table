<template>
  <el-table-column v-bind="$attrs"
    v-on="$listeners"
    :prop="column.prop"
    :label="column.label"
    :type="column.type"
    :index="column.index"
    :column-key="column.columnKey"
    :width="column.width"
    :min-width="column.minWidth"
    :fixed="column.fixed"
    :render-header="column.renderHeader"
    :sortable="column.sortable || false"
    :sort-method="column.sortMethod"
    :sort-by="column.sortBy"
    :sort-orders="column.sortOrders"
    :resizable="column.resizable || true"
    :formatter="column.formatter"
    :show-overflow-tooltip="column.showOverflowTooltip || false"
    :align="column.align || align || 'left'"
    :header-align="column.headerAlign || headerAlign || column.align || align || 'left'"
    :class-name="column.className"
    :label-class-name="column.labelClassName"
    :selectable="column.selectable"
    :reserve-selection="column.reserveSelection || false"
    :filters="column.filters"
    :filter-placement="column.filterPlacement"
    :filter-multiple="column.filterMultiple"
    :filter-method="column.filterMethod"
    :filtered-value="column.filteredValue">

    <template slot="header"
      slot-scope="scope">
      <hmf-render :prop-column="column"
        :scope="scope"
        :render="column.renderHeader">
      </hmf-render>
    </template>

    <template slot-scope="scope">
      <hmf-render :prop-column="column"
        :scope="scope"
        :render="column.render">
      </hmf-render>
    </template>

    <template v-if="column.children">
      <hmf-column v-for="(col, index) in column.children"
        :key="index"
        :column="col">
      </hmf-column>
    </template>
  </el-table-column>
</template>

<script>
import HmfRender from './hmf-render'
import forced from './forced.js'
export default {
  name: 'HmfColumn',
  props: {
    column: Object,
    headerAlign: String,
    align: String
  },
  components: {
    HmfRender
  },
  created () {
    if (this.column.type) {
      this.column.renderHeader = forced[this.column.type].renderHeader
      this.column.render = this.column.render || forced[this.column.type].renderCell
    }
    if (this.column.formatter) {
      this.column.render = (h, scope) => {
        return <span>{ scope.column.formatter(scope.row, scope.column, scope.row, scope.$index) }</span>
      }
    }
    if (!this.column.render) {
      this.column.render = (h, scope) => {
        return <span>{ scope.row[scope.column.property] }</span>
      }
    }
    if (!this.column.renderHeader) {
      this.column.renderHeader = (h, scope) => {
        return <span>{ scope.column.label }</span>
      }
    }
  }
}
</script>
