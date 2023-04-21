<template>
    <div class="table-wrapper">
        <div class="resizer">
            <ui5-table class="demo-table" id="table">
                <!-- Columns -->
                <ui5-table-column slot="columns" min-width="800">
                    <span style="line-height: 1.4rem">Product</span>
                </ui5-table-column>

                <ui5-table-column slot="columns" min-width="800">
                    <span style="line-height: 1.4rem">Supplier</span>
                </ui5-table-column>

                <ui5-table-column slot="columns" min-width="600" popin-text="Dimensions" demand-popin
                    class="table-header-text-alignment">
                    <span style="line-height: 1.4rem">Dimensions</span>
                </ui5-table-column>

                <ui5-table-column slot="columns" min-width="600" popin-text="Weight" demand-popin
                    class="table-header-text-alignment">
                    <span style="line-height: 1.4rem">Weight</span>
                </ui5-table-column>

                <ui5-table-column slot="columns" min-width="800" class="table-header-text-alignment">
                    <span style="line-height: 1.4rem">Price</span>
                </ui5-table-column>

                <ui5-table-row>
                    <ui5-table-cell>
                        <div class="double-line-content">
                            <span><b>Notebook Basic 15</b></span>
                            <span style="margin-top: 0.5rem">HT-1000</span>
                        </div>
                    </ui5-table-cell>
                    <ui5-table-cell>
                        <span>Very Best Screens</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span>30 x 18 x 3cm</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span style="color: #2b7c2b"><b>4.2</b>KG</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span><b>956</b>EUR</span>
                    </ui5-table-cell>
                </ui5-table-row>

                <ui5-table-row>
                    <ui5-table-cell>
                        <div class="double-line-content">
                            <span><b>Notebook Basic 175</b></span>
                            <span style="margin-top: 0.5rem">HT-1001</span>
                        </div>
                    </ui5-table-cell>
                    <ui5-table-cell>
                        <span>Very Best Screens</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span>29 x 17 x 3.1cm</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span style="color: #2b7c2b"><b>4.5</b>KG</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span><b>1249</b>EUR</span>
                    </ui5-table-cell>
                </ui5-table-row>

                <ui5-table-row>
                    <ui5-table-cell>
                        <div class="double-line-content">
                            <span><b>Notebook Basic 18</b></span>
                            <span style="margin-top: 0.5rem">HT-1002</span>
                        </div>
                    </ui5-table-cell>
                    <ui5-table-cell>
                        <span>Very Best Screens</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span>28 x 19 x 2.5cm</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span style="color: #2b7c2b"><b>4.2</b>KG</span>
                    </ui5-table-cell>
                    <ui5-table-cell style="text-align: right">
                        <span><b>1570</b>EUR</span>
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
</script>

<style>
.table-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 2rem 0;
    box-sizing: border-box;
    background-color: var(--sapObjectHeader_Background);
}

.resizer {
    min-width: 100%;
    width: auto;
    resize: horizontal;
    overflow-x: auto;
}

.resizer::-webkit-resizer {
    display: none;
}

.demo-table {
    min-width: 50%;
    width: auto;
}

ui5-table-column::part(column) {
    overflow: hidden;
    resize: horizontal;
}
</style>
