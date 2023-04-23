<template>
    <div class="wrapper selection-wrapper">
        <ui5-list mode='MultiSelect' header-text="Column selection" @selection-change="selectionChange($event)">
            <ui5-li v-for="col, i in columns" :key="`li_${i}`" :id="`li_${i}`" :selected="columns[i].active">
                {{ col.header }}
            </ui5-li>
        </ui5-list>
    </div>
    <div class="wrapper">
        <div class="table-container">
            <ui5-table class="demo-table" ref="table">
                <ui5-table-column v-for="col, i in activeColumns" :key="`col_${i}`" :ref="`col_${i}`" slot="columns"
                    :style="`width: ${col.width}px`">
                    <span class="column-text">{{ col.header }}</span>
                    <div class="resizer" draggable="true" @dragstart="dragstart($event, i)" @drag="drag($event)"
                        @dragend="dragend" :style="resizerStyle" :class="[dragging.index === i ? 'dragging' : '']">
                    </div>
                </ui5-table-column>
                <ui5-table-row v-for="val, i_val in values" :key="`row_${i_val}}`">
                    <ui5-table-cell v-for="col, i_col in activeColumns" :key="`row_${i_val}_${i_col}`">
                        <span class="cell-text">{{ val[col.key] }}</span>
                    </ui5-table-cell>
                </ui5-table-row>
            </ui5-table>
        </div>
    </div>
</template>

<script>
import "@ui5/webcomponents/dist/Button.js";
import "@ui5/webcomponents/dist/Table.js";
import "@ui5/webcomponents/dist/TableColumn.js";
import "@ui5/webcomponents/dist/TableRow.js";
import "@ui5/webcomponents/dist/TableCell.js";

export default {
    data() {
        return {
            columns: [
                { key: "product", header: "Product", width: 120, active: true },
                { key: "supplier", header: "Supplier", width: 120, active: true },
                { key: "dimensions", header: "Dimensions", width: 120, active: true },
                { key: "weight", header: "Weight", width: 120, active: true },
                { key: "price", header: "Price", width: 120, active: true },
            ],
            values: [
                { product: "Notebook Basic 15HT-1000", supplier: "Very Best Screens", dimensions: "30 x 18 x 3cm", weight: "4.2KG", price: "956EUR" },
                { product: "Notebook Basic 175HT-1001", supplier: "Very Best Screens", dimensions: "29 x 17 x 3.1cm", weight: "4.5KG", price: "1249EUR" },
                { product: "Notebook Basic 18HT-1002", supplier: "Very Best Screens", dimensions: "28 x 19 x 2.5cm", weight: "4.2KG", price: "1570EUR" }
            ],
            dragging: {},
        }
    },
    mounted() {
        // 初期表示の列幅を文字数に応じて調整する
        this.columns.forEach((col, i) => {
            const width = Math.max(...this.values.map(val => val[col.key].length)) * 10
            this.columns[i].width = Math.max(this.columns[i].width, width)
        })
    },
    computed: {
        resizerStyle() {
            const height = `calc(${(this.values.length + 1) * 100}%)`
            return { height }
        },
        activeColumns() {
            return this.columns.filter(col => col.active)
        }
    },
    methods: {
        selectionChange(e) {
            console.log(e.detail.targetItem.id, e.detail.targetItem._state.selected)
            this.columns[e.detail.targetItem.id.match(/\d+/).pop()].active = e.detail.targetItem._state.selected
        },
        dragstart(e, i) {
            this.dragging.index = i
            this.dragging.start = e.clientX
            this.dragging.width = this.$refs[`col_${i}`][0].shadowRoot.childNodes[1].clientWidth
            this.dragging.tableWidth = this.$refs['table'].clientWidth
        },
        drag(e) {
            if (e.clientX) {
                const dx = e.clientX - this.dragging.start
                this.$refs['table'].style.width = `${this.dragging.tableWidth + dx}px`
                this.$refs[`col_${this.dragging.index}`][0].shadowRoot.childNodes[1].style.width = `${this.dragging.width + dx}px`
            }
        },
        dragend() {
            this.dragging = {}
        }
    }
}
</script>

<style lang="scss" scoped>
.wrapper {
    display: flex;
    align-items: center;
    margin: 2rem 0;
    box-sizing: border-box;
    background-color: var(--sapObjectHeader_Background);
}

.selection-wrapper {
    width: 500px;
}

.table-container {
    overflow-x: auto;
}

ui5-table-column::part(column) {
    position: relative;
}

column-text {
    line-height: 1.4rem;
}

.resizer {
    position: absolute;
    width: 10px;
    background-color: none;
    right: 0;
    top: 1px;
    cursor: col-resize;
    user-select: none;

    &::after {
        content: '';
        display: block;
        width: var(--ui5_table_header_row_border_width);
        height: 100%;
        background-color: var(--sapList_BorderColor);
        position: relative;
        margin: auto;


    }

    &:hover::after,
    &.dragging::after {
        width: var(--sapField_InformationBorderWidth);
        background-color: var(--sapList_SelectionBorderColor);
    }
}

ui5-table-cell {
    max-width: 0;
    text-overflow: ellipsis;
    white-space: nowrap;
}
</style>
