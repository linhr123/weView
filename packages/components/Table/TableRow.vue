<script lang="ts">
export default {
  name: 'TableRow',
}
</script>

<script lang="ts" setup>
import './assets/font.scss'
import { Recordable } from './typing'
import { computed, ref, StyleValue, useSlots, WritableComputedRef } from 'vue'
import WvCheckbox from './checkbox/index.vue'
import WvTooltip from './tooltip/index.vue'
import WvRadio from './radio/index.vue'
import { TableEmit } from './typing'

export interface TableRowProps {
  index: number
  indentSize: number
  currentIndentSize: number
  expandSpace: boolean
  expandIndex: number
  selectedKeys: Recordable[]
  selectedKey: string
  tableColumnKeys: Recordable[]
  childrenColumnName?: string
  columnSlotNames: String[]
  page?: any
  columns: Recordable[]
  checkbox?: boolean
  cellClassName: string | Function
  cellStyle: string | Function
  rowClassName: string | Function
  rowStyle: string | Function
  id: string
  data: any
  spanMethod: Function
  defaultExpandAll: boolean
  expandKeys: Recordable[]
  getCheckboxProps: Function
  getRadioProps: Function
}

const slot = useSlots()

const emit = defineEmits(TableEmit)

const props = withDefaults(defineProps<TableRowProps>(), {
  checkbox: false,
  childrenColumnName: 'children',
  cellStyle: '',
  cellClassName: '',
})

const tableExpandAll = ref(props.defaultExpandAll)

const tableExpandKeys: WritableComputedRef<Recordable[]> = computed({
  get() {
    return [...props.expandKeys]
  },
  set(val) {
    emit('update:expandKeys', val)
  },
})

const tableSelectedKeys: WritableComputedRef<Recordable[]> = computed({
  get() {
    return [...props.selectedKeys]
  },
  set(val) {
    emit('update:selectedKeys', val)
  },
})

const tableSelectedKey: WritableComputedRef<string> = computed({
  get() {
    return props.selectedKey
  },
  set(val) {
    emit('update:selectedKey', val)
  },
})

const isExpand: WritableComputedRef<any> = computed({
  get() {
    return tableExpandAll.value
      ? true
      : tableExpandKeys.value.includes(props.data[props.id])
  },
  set(val) {
    let newTableExpandKeys = [...tableExpandKeys.value]
    if (!val) {
      newTableExpandKeys.splice(
        newTableExpandKeys.indexOf(props.data[props.id]),
        1
      )
    } else {
      newTableExpandKeys.push(props.data[props.id])
    }
    tableExpandAll.value = false
    tableExpandKeys.value = newTableExpandKeys
  },
})

const rowClick = function (data: any, evt: MouseEvent) {
  emit('row', data, evt)
}

const rowDoubleClick = function (data: any, evt: MouseEvent) {
  emit('row-double', data, evt)
}

const rowContextmenu = function (data: any, evt: MouseEvent) {
  emit('row-contextmenu', data, evt)
}

const expandIconType = computed(() => {
  return isExpand.value ? 'wv-icon-subtraction' : 'wv-icon-addition'
})

const handleExpand = () => {
  isExpand.value = !isExpand.value
}

const renderCellStyle = (
  row: any,
  column: any,
  rowIndex: number,
  columnIndex: number
) => {
  if (typeof props.cellStyle === 'string') {
    return props.cellStyle
  }
  return props.cellStyle(row, column, rowIndex, columnIndex)
}

const renderCellClassName = (
  row: any,
  column: any,
  rowIndex: number,
  columnIndex: number
) => {
  if (typeof props.cellClassName === 'string') {
    return props.cellClassName
  }
  return props.cellClassName(row, column, rowIndex, columnIndex)
}

const renderRowStyle = (data: any, index: number) => {
  if (typeof props.rowStyle === 'string') {
    return props.rowStyle
  }
  return props.rowStyle(data, index)
}

const renderRowClassName = (data: any, index: number) => {
  if (typeof props.rowClassName === 'string') {
    return props.rowClassName
  }
  return props.rowClassName(data, index)
}

const childrenIndentSize = computed(
  () => props.currentIndentSize + props.indentSize
)

const renderFixedStyle = (column: any, columnIndex: number) => {
  if (column.fixed) {
    if (column.fixed == 'left') {
      var left = 0
      for (let i = 0; i < columnIndex; i++) {
        if (
          props.columns[i].fixed &&
          props.columns[i].fixed == 'left' &&
          props.tableColumnKeys.includes(props.columns[i].key)
        ) {
          left = left + Number(props.columns[i]?.width?.replace('px', ''))
        }
      }
      return { left: `${left}px` } as StyleValue
    } else {
      var right = 0
      for (let i = columnIndex + 1; i < props.columns.length; i++) {
        if (
          props.columns[i].fixed &&
          props.columns[i].fixed == 'right' &&
          props.tableColumnKeys.includes(props.columns[i].key)
        ) {
          right = right + Number(props.columns[i]?.width?.replace('px', ''))
        }
      }
      return { right: `${right}px` } as StyleValue
    }
  } else {
    var isLast = true
    for (let i = columnIndex + 1; i < props.columns.length; i++) {
      if (
        props.columns[i].fixed == undefined &&
        props.tableColumnKeys.includes(props.columns[i].key)
      ) {
        isLast = false
      }
    }
    return isLast ? ({ 'border-right': 'none' } as StyleValue) : {}
  }
  // return {} as StyleValue
}

const renderFixedClassName = (column: any, columnIndex: number) => {
  if (column.fixed) {
    if (column.fixed == 'left') {
      var left = true
      for (let i = columnIndex + 1; i < props.columns.length; i++) {
        if (
          props.columns[i].fixed &&
          props.columns[i].fixed == 'left' &&
          props.tableColumnKeys.includes(props.columns[i].key)
        ) {
          left = false
        }
      }
      return left ? `wv-table-fixed-left-last` : ''
    } else {
      var right = true
      for (let i = 0; i < columnIndex; i++) {
        if (
          props.columns[i].fixed &&
          props.columns[i].fixed == 'right' &&
          props.tableColumnKeys.includes(props.columns[i].key)
        ) {
          right = false
        }
      }
      return right ? `wv-table-fixed-right-first` : ''
    }
  }
}

const spanMethodAttr = (
  row: any,
  column: any,
  rowIndex: number,
  columnIndex: number
) => {
  const attrs = props.spanMethod(row, column, rowIndex, columnIndex)
  if (attrs instanceof Array) {
    return { rowspan: attrs[0], colspan: attrs[1] }
  } else if (attrs instanceof Object) {
    return attrs
  } else {
    return { rowspan: 1, colspan: 1 }
  }
}

const isAutoShow = (
  row: any,
  column: any,
  rowIndex: number,
  columnIndex: number
) => {
  const attrs = spanMethodAttr(row, column, rowIndex, columnIndex)
  if (attrs.colspan == 0 && attrs.rowspan == 0) {
    return false
  } else {
    return true
  }
}

const radioProps = props.getRadioProps(props.data, props.index)
const checkboxProps = props.getCheckboxProps(props.data, props.index)
</script>

<template>
  <tr
    :style="[renderRowStyle(data, index)]"
    :class="[renderRowClassName(data, index)]"
    @click.stop="rowClick(data, $event)"
    @dblclick.stop="rowDoubleClick(data, $event)"
    @contextmenu.stop="rowContextmenu(data, $event)"
  >
    <template v-for="(column, columnIndex) in columns" :key="columnIndex">
      <template v-if="tableColumnKeys.includes(column.key)">
        <template v-if="column.type">
          <template v-if="column.type == 'radio'">
            <td
              class="wv-table-cell wv-table-cell-radio"
              v-if="isAutoShow(data, column, index, columnIndex)"
              :colspan="
                spanMethodAttr(data, column, index, columnIndex).colspan
              "
              :rowspan="
                spanMethodAttr(data, column, index, columnIndex).rowspan
              "
              :style="[
                {
                  textAlign: column.align,
                  whiteSpace: column.ellipsisTooltip ? 'nowrap' : 'normal',
                },
                renderFixedStyle(column, columnIndex),
                renderCellStyle(data, column, index, columnIndex),
              ]"
              :class="[
                renderFixedClassName(column, columnIndex),
                renderCellClassName(data, column, index, columnIndex),
                column.fixed ? `wv-table-fixed-${column.fixed}` : '',
              ]"
            >
              <span
                v-if="expandSpace && columnIndex === expandIndex"
                :style="{ 'margin-right': currentIndentSize + 'px' }"
              />

              <span
                v-if="
                  expandSpace &&
                  !data[childrenColumnName] &&
                  !slot.expand &&
                  columnIndex === expandIndex
                "
                class="wv-table-cell-expand-icon-spaced"
              />

              <i
                v-if="
                  (slot.expand || data[childrenColumnName]) &&
                  columnIndex === expandIndex
                "
                :class="'wv-icon wv-table-cell-expand-icon ' + expandIconType"
                @click="handleExpand"
              />

              <wv-radio
                v-model="tableSelectedKey"
                v-bind="radioProps"
                :value="data[id]"
              />
            </td>
          </template>

          <template v-if="column.type == 'checkbox'">
            <td
              class="wv-table-cell wv-table-cell-checkbox"
              v-if="isAutoShow(data, column, index, columnIndex)"
              :colspan="
                spanMethodAttr(data, column, index, columnIndex).colspan
              "
              :rowspan="
                spanMethodAttr(data, column, index, columnIndex).rowspan
              "
              :style="[
                {
                  textAlign: column.align,
                  whiteSpace: column.ellipsisTooltip ? 'nowrap' : 'normal',
                },
                renderFixedStyle(column, columnIndex),
                renderCellStyle(data, column, index, columnIndex),
              ]"
              :class="[
                renderFixedClassName(column, columnIndex),
                renderCellClassName(data, column, index, columnIndex),
                column.fixed ? `wv-table-fixed-${column.fixed}` : '',
              ]"
            >
              <span
                v-if="expandSpace && columnIndex === expandIndex"
                :style="{ 'margin-right': currentIndentSize + 'px' }"
              />

              <span
                v-if="
                  expandSpace &&
                  !data[childrenColumnName] &&
                  !slot.expand &&
                  columnIndex === expandIndex
                "
                class="wv-table-cell-expand-icon-spaced"
              />

              <i
                v-if="
                  (slot.expand || data[childrenColumnName]) &&
                  columnIndex === expandIndex
                "
                :class="'wv-icon wv-table-cell-expand-icon ' + expandIconType"
                @click="handleExpand"
              />

              <wv-checkbox
                v-model="tableSelectedKeys"
                v-bind="checkboxProps"
                :value="data[id]"
                skin="primary"
              />
            </td>
          </template>

          <template v-if="column.type == 'number'">
            <td
              class="wv-table-cell wv-table-cell-number"
              v-if="isAutoShow(data, column, index, columnIndex)"
              :colspan="
                spanMethodAttr(data, column, index, columnIndex).colspan
              "
              :rowspan="
                spanMethodAttr(data, column, index, columnIndex).rowspan
              "
              :style="[
                {
                  textAlign: column.align,
                  whiteSpace: column.ellipsisTooltip ? 'nowrap' : 'normal',
                },
                renderFixedStyle(column, columnIndex),
                renderCellStyle(data, column, index, columnIndex),
              ]"
              :class="[
                renderFixedClassName(column, columnIndex),
                renderCellClassName(data, column, index, columnIndex),
                column.fixed ? `wv-table-fixed-${column.fixed}` : '',
              ]"
            >
              <span
                v-if="expandSpace && columnIndex === expandIndex"
                :style="{ 'margin-right': currentIndentSize + 'px' }"
              />

              <span
                v-if="
                  expandSpace &&
                  !data[childrenColumnName] &&
                  !slot.expand &&
                  columnIndex === expandIndex
                "
                class="wv-table-cell-expand-icon-spaced"
              />

              <i
                v-if="
                  (slot.expand || data[childrenColumnName]) &&
                  columnIndex === expandIndex
                "
                :class="'wv-icon wv-table-cell-expand-icon ' + expandIconType"
                @click="handleExpand"
              />

              {{
                page?.current
                  ? (page?.current - 1) * page?.limit + index + 1
                  : index + 1
              }}
            </td>
          </template>
        </template>

        <template v-else>
          <template v-if="column.customSlot">
            <td
              class="wv-table-cell"
              v-if="isAutoShow(data, column, index, columnIndex)"
              :colspan="
                spanMethodAttr(data, column, index, columnIndex).colspan
              "
              :rowspan="
                spanMethodAttr(data, column, index, columnIndex).rowspan
              "
              :style="[
                {
                  textAlign: column.align,
                  whiteSpace: column.ellipsisTooltip ? 'nowrap' : 'normal',
                },
                renderFixedStyle(column, columnIndex),
                renderCellStyle(data, column, index, columnIndex),
              ]"
              :class="[
                renderFixedClassName(column, columnIndex),
                renderCellClassName(data, column, index, columnIndex),
                column.fixed ? `wv-table-fixed-${column.fixed}` : '',
              ]"
            >
              <div
                style="display: flex"
                :style="[
                  { textAlign: column.align, justifyContent: column.align },
                ]"
              >
                <span
                  v-if="expandSpace && columnIndex === expandIndex"
                  :style="{ 'margin-right': currentIndentSize + 'px' }"
                />

                <span
                  v-if="
                    expandSpace &&
                    !data[childrenColumnName] &&
                    !slot.expand &&
                    columnIndex === expandIndex
                  "
                  class="wv-table-cell-expand-icon-spaced"
                />

                <i
                  v-if="
                    (slot.expand || data[childrenColumnName]) &&
                    columnIndex === expandIndex
                  "
                  :class="'wv-icon wv-table-cell-expand-icon ' + expandIconType"
                  @click="handleExpand"
                />

                <wv-tooltip v-if="column.ellipsisTooltip" :isAutoShow="true">
                  <slot
                    :row="data"
                    :data="data"
                    :column="column"
                    :rowIndex="index"
                    :columnIndex="columnIndex"
                    :name="column.customSlot"
                  />
                  <template #content>
                    <slot
                      :row="data"
                      :data="data"
                      :column="column"
                      :rowIndex="index"
                      :columnIndex="columnIndex"
                      :name="column.customSlot"
                    />
                  </template>
                </wv-tooltip>

                <slot
                  v-else
                  :row="data"
                  :data="data"
                  :column="column"
                  :rowIndex="index"
                  :columnIndex="columnIndex"
                  :name="column.customSlot"
                />
              </div>
            </td>
          </template>

          <template v-else>
            <td
              class="wv-table-cell"
              v-if="isAutoShow(data, column, index, columnIndex)"
              :colspan="
                spanMethodAttr(data, column, index, columnIndex).colspan
              "
              :rowspan="
                spanMethodAttr(data, column, index, columnIndex).rowspan
              "
              :style="[
                {
                  textAlign: column.align,
                  whiteSpace: column.ellipsisTooltip ? 'nowrap' : 'normal',
                },
                renderFixedStyle(column, columnIndex),
                renderCellStyle(data, column, index, columnIndex),
              ]"
              :class="[
                renderFixedClassName(column, columnIndex),
                renderCellClassName(data, column, index, columnIndex),
                column.fixed ? `wv-table-fixed-${column.fixed}` : '',
              ]"
            >
              <div
                style="display: flex"
                :style="[
                  { textAlign: column.align, justifyContent: column.align },
                ]"
              >
                <span
                  v-if="expandSpace && columnIndex === expandIndex"
                  :style="{ 'margin-right': currentIndentSize + 'px' }"
                />

                <span
                  v-if="
                    expandSpace &&
                    !data[childrenColumnName] &&
                    !slot.expand &&
                    columnIndex === expandIndex
                  "
                  class="wv-table-cell-expand-icon-spaced"
                />

                <i
                  v-if="
                    (slot.expand || data[childrenColumnName]) &&
                    columnIndex === expandIndex
                  "
                  :class="'wv-icon wv-table-cell-expand-icon ' + expandIconType"
                  @click="handleExpand"
                />

                <wv-tooltip
                  v-if="column.ellipsisTooltip"
                  :content="data[column.key]"
                  :isAutoShow="true"
                >
                  {{ data[column.key] }}
                </wv-tooltip>

                <span v-else> {{ data[column.key] }} </span>
              </div>
            </td>
          </template>
        </template>
      </template>
    </template>
  </tr>

  <tr class="wv-table-cell-expand" v-if="slot.expand && isExpand">
    <td class="wv-table-cell" :colspan="columns.length">
      <slot name="expand" :data="data" :row="data" />
    </td>
  </tr>

  <template v-if="data[childrenColumnName] && isExpand">
    <template
      v-for="(children, childrenIndex) in data[childrenColumnName]"
      :key="childrenIndex"
    >
      <table-row
        :id="id"
        :data="children"
        :index="childrenIndex"
        :page="page"
        :columns="columns"
        :columnSlotNames="columnSlotNames"
        :indent-size="indentSize"
        :current-indent-size="childrenIndentSize"
        :checkbox="checkbox"
        :tableColumnKeys="tableColumnKeys"
        :expandSpace="expandSpace"
        :expandIndex="expandIndex"
        :cellStyle="cellStyle"
        :cellClassName="cellClassName"
        :rowStyle="rowStyle"
        :rowClassName="rowClassName"
        :spanMethod="spanMethod"
        :defaultExpandAll="defaultExpandAll"
        :getCheckboxProps="getCheckboxProps"
        :getRadioProps="getRadioProps"
        @row="rowClick"
        @row-double="rowDoubleClick"
        @row-contextmenu="rowContextmenu"
        v-model:expandKeys="tableExpandKeys"
        v-model:selectedKeys="tableSelectedKeys"
        v-model:selectedKey="tableSelectedKey"
      >
        <template
          v-for="name in columnSlotNames"
          #[name]="slotProp: {
            data: any,
            column: any,
            row: any,
            rowIndex: number,
            columnIndex: number,
          }"
        >
          <slot
            :name="name"
            :row="slotProp.data"
            :data="slotProp.data"
            :column="slotProp.column"
            :rowIndex="slotProp.rowIndex"
            :columnIndex="slotProp.columnIndex"
          />
        </template>
        <template
          v-if="slot.expand"
          #expand="slotProp: { data: any, row: any }"
        >
          <slot name="expand" :data="slotProp.data" :row="slotProp.row" />
        </template>
      </table-row>
    </template>
  </template>
</template>
