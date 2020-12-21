<template>
  <div>
    <button @click="resetGrid">Reset Grid</button>
    <div v-if="showGrid" style="height: 100vh">
      <ag-grid-vue
        style="width: 100%; height: 100%"
        class="ag-theme-alpine"
        id="myGrid"
        :gridOptions="gridOptions"
        @grid-ready="onGridReady"
        :columnDefs="columnDefs"
        :defaultColDef="defaultColDef"
        :enableRangeSelection="true"
        :rowData="rowData"
        :sideBar="true"
      ></ag-grid-vue>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable vue/no-unused-components */

import { Component, Vue, Prop } from "vue-property-decorator";
import { AgGridVue } from "@ag-grid-community/vue";
import {
  ModuleRegistry,
  GridOptions,
  ColDef,
  GridApi,
  ColumnApi,
  GridReadyEvent,
} from "@ag-grid-community/core";

import { ClientSideRowModelModule } from "@ag-grid-community/client-side-row-model";
import { SideBarModule } from "@ag-grid-enterprise/side-bar";
import { ColumnsToolPanelModule } from "@ag-grid-enterprise/column-tool-panel";

ModuleRegistry.registerModules([
  ClientSideRowModelModule,
  SideBarModule,
  ColumnsToolPanelModule,
]);

@Component({
  name: "App",
  components: {
    AgGridVue,
  },
})
export default class App extends Vue {
  @Prop()
  gridOptions: GridOptions | null | undefined;
  @Prop()
  columnDefs: ColDef[] | null | undefined;
  @Prop()
  defaultColDef: ColDef | null | undefined;
  @Prop()
  gridApi: GridApi | null | undefined;
  @Prop()
  gridColumnApi: ColumnApi | null | undefined;

  @Prop()
  rowData: any[] | null | undefined;
  @Prop()
  rowData1: any[] | null | undefined;
  @Prop()
  rowData2: any[] | null | undefined;

  private showGrid: boolean = true;
  private appInit: boolean = false;

  public beforeMount() {
    this.gridOptions = {};
    this.columnDefs = [
      {
        field: "athlete",
        minWidth: 150,
      },
      {
        field: "age",
        maxWidth: 90,
      },
      {
        field: "country",
        minWidth: 150,
      },
      {
        field: "year",
        maxWidth: 90,
      },
      {
        field: "date",
        minWidth: 150,
      },
      {
        field: "sport",
        minWidth: 150,
      },
      { field: "gold" },
      { field: "silver" },
      { field: "bronze" },
      { field: "total" },
    ];
    this.defaultColDef = {
      flex: 1,
      minWidth: 100,
    };
  }
  mounted() {
    this.gridApi = this.gridOptions!.api;
    this.gridColumnApi = this.gridOptions!.columnApi;
  }
  private onGridReady(params: GridReadyEvent) {
    const httpRequest = new XMLHttpRequest();
    const updateData = (data: any[]) => {
      this.rowData1 = data.slice(0, 10);
      this.rowData2 = data.slice(10, 20);
      this.rowData = this.rowData1;
    };

    if (!this.appInit) {
      httpRequest.open(
        "GET",
        "https://raw.githubusercontent.com/ag-grid/ag-grid/master/grid-packages/ag-grid-docs/src/olympicWinnersSmall.json"
      );
      httpRequest.send();
      httpRequest.onreadystatechange = () => {
        if (httpRequest.readyState === 4 && httpRequest.status === 200) {
          updateData(JSON.parse(httpRequest.responseText));
        }
      };
      this.appInit = true;
    }
  }
  private resetGrid() {
    this.showGrid = false;
    console.log(this.rowData![0].athlete, this.rowData2![0].athlete);
    this.rowData =
      this.rowData![0].athlete === "Michael Phelps"
        ? this.rowData2
        : this.rowData1;
    setTimeout(() => {
      this.showGrid = true;
    }, 10);
  }
}
</script>

<style>
@import "../node_modules/@ag-grid-community/core/dist/styles/ag-grid.css";
@import "../node_modules/@ag-grid-community/core/dist/styles/ag-theme-alpine.css";
</style>
