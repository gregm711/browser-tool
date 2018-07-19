<template>
  <div class="hello">

    <div class="container">
  <div class="columns">
    <div class="menu-container column">
      <p class="menu-label">
        Example list
      </p>
      <div class="menu"></div>
    </div>
    <div class="demo column is-two-thirds">
      <div id="myGrid" style="width:100%;height:500px;" class="slickgrid-container"></div>
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
  name: 'Home',
  data () {
    return {

    }
  },
  methods:{
    createSlickGridTable(){
      var options = {
        forceFitColumns: true,
        headerRowHeight: 40,
        rowHeight: 60
    };

    var positions = [
{ "security": "Apple", "symbol": "AAPL", "quantity": 100, "lastTrade": 700 },
{ "security": "Goldman Sachs Group Inc.", "symbol": "GS", "quantity": 500, "lastTrade": 116.32 },
{ "security": "Chevron Corporation", "symbol": "CVX", "quantity": 300, "lastTrade": 118.01 },
{ "security": "Google", "symbol": "GOOG", "quantity": 200, "lastTrade": 55 },
{ "security": "Amazon.com Inc.", "symbol": "AMZN", "quantity": 100, "lastTrade": 255.12 },
{ "security": "Microsoft, Inc.", "symbol": "MSFT", "quantity": 200, "lastTrade": 71.55 },
{ "security": "Coca-Cola Enterprises Inc.", "symbol": "CCE", "quantity": 500, "lastTrade": 31.22 },
{ "security": "Adobe Systems Inc.", "symbol": "ADBE", "quantity": 300, "lastTrade": 33.49 },
{ "security": "Colgate-Palmolive Company", "symbol": "CL", "quantity": 300, "lastTrade": 106.79 },
{ "security": "Dell Inc.", "symbol": "DELL", "quantity": 300, "lastTrade": 10.14 },
{ "security": "Cisco", "symbol": "CSCO", "quantity": 600, "lastTrade": 60 },
{ "security": "The Walt Disney Company", "symbol": "DIS", "quantity": 300, "lastTrade": 52.81 },
{ "security": "The Dow Chemical Company", "symbol": "DOW", "quantity": 300, "lastTrade": 30.20 },
{ "security": "eBay Inc.", "symbol": "EBAY", "quantity": 300, "lastTrade": 49.27 },
{ "security": "EMC Corporation", "symbol": "EMC", "quantity": 300, "lastTrade": 27.74 },
{ "security": "The Boeing Company", "symbol": "BA", "quantity": 600, "lastTrade": 69.53 },
{ "security": "Kellogg Company", "symbol": "K", "quantity": 500, "lastTrade": 70 },
{ "security": "General Electric", "symbol": "GE", "quantity": 1000, "lastTrade": 22.53 },
{ "security": "Akamai Technologies Inc.", "symbol": "AKAM", "quantity": 500, "lastTrade": 38.33 },
{ "security": "Eastman Chemical Company", "symbol": "EMN", "quantity": 250, "lastTrade": 56.37 },
{ "security": "Bank of America Corporation", "symbol": "BAC", "quantity": 2000, "lastTrade": 9.10 },
{ "security": "Raytheon Company", "symbol": "RTN", "quantity": 200, "lastTrade": 57.93 },
{ "security": "Advanced Micro Devices Inc.", "symbol": "AMD", "quantity": 2000, "lastTrade": 3.49 },
{ "security": "Electronic Arts Inc.", "symbol": "EA", "quantity": 1000, "lastTrade": 13.27 },
{ "security": "Northeast Utilities", "symbol": "NU", "quantity": 100, "lastTrade": 38.02 },
{ "security": "Radioshack Corporation", "symbol": "RSH", "quantity": 500, "lastTrade": 3.08 },
{ "security": "Yahoo! Inc.", "symbol": "YHOO", "quantity": 200, "lastTrade": 20 },
{ "security": "Urban Outfitters", "symbol": "URBN", "quantity": 500, "lastTrade": 10 },
{ "security": "The Loosers 1", "symbol": "LSR1", "quantity": 2500, "lastTrade": 1.28 },
{ "security": "The Loosers 2", "symbol": "LSR2", "quantity": 500, "lastTrade": 0 },
{ "security": "Cash", "symbol": "CASH", "quantity": 10000, "lastTrade": 1 }
];

    var columns = [
{ name: 'Security',   field: 'security',  id: 'security',  sortable: true },
{ name: 'Symbol',     field: 'symbol',    id: 'symbol',    sortable: true },
{ name: 'Quantity',   field: 'quantity',  id: 'quantity',  sortable: true },
{ name: 'Last Trade', field: 'lastTrade', id: 'lastTrade', sortable: true }
];


        const grid = new Grid('#myGrid', positions, columns, options);
         grid.resizeCanvas();
    }

  },

  mounted: function () {
    this.createSlickGridTable();

    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

</style>
