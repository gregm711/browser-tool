<template>
  <div class="table-viewer">
    <div class="container">
  <div class="columns">
    <div class="menu-container column">
      <div class="menu"></div>
    </div>
    <div class="demo column is-two-thirds">
      <div id="myGrid" style="width:100%;height:600px;" class="slickgrid-container"></div>
    </div>
    <div class="controls column"></div>
  </div>
</div>
  </div>
</template>

<script>
import Papa from 'papaparse'
import sql from 'sql.js'
import { Grid, Data, Formatters } from 'slickgrid-es6';
export default {
  name: 'TableViewer',
  data () {
    return {
    }
  },
  props: {
  rows: Array,
  columns: Array,
},
watch: {
    rows: function (val) {
      console.log("new values")
      this.createSlickGridTable();
    }
  },

  methods:{
    createSlickGridTable(){
      const options = {
        forceFitColumns: false,
        fullWidthRows: true,
        showHeaderRow: true,
        };
    var positions = this.rows;
    var columns = this.columns;
    var formattedColumns = [];
    for (var i =0; i < columns.length; i++){
      var dict = {name: columns[i],  field: columns[i],  id: columns[i],  selectable: true }
      formattedColumns.push(dict)
    }
        const grid = new Grid('#myGrid', positions, formattedColumns, options);

    }

  },



}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped="true">
.table-viewer{
  width: 70%;
  margin: 0 auto;
  margin-top: 8rem;

}


</style>
