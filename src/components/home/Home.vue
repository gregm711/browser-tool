<template>
    <div class="home">
        <h1>Upload EXWRK csv</h1>
        <div style="margin-top: 3rem;"></div>

        <input id="fileInput" class="input" type="file" @change="upload" style="padding: 2rem;">
        <div class="wrapper" v-if="this.rows.length > 0">
        <div class="input-group mb-3">
            <input type="text" class="form-control" placeholder="Enter SQL Query" aria-label="SQL Query" v-model="query" aria-describedby="basic-addon2">
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button" @click="executeQuery(query)">Enter Query</button>
            </div>
        </div>
        <div class="input-group mb-3">
            <input type="text" class="form-control" placeholder="Enter slope" aria-label="SQL Query" @change="executeQuery" aria-describedby="basic-addon2">
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button">Enter the correct slope</button>
            </div>
        </div>
        <div>
          <h4>Please select from the available columns</h4>
            <b-form-group >
                <b-form-checkbox-group style="text-align: left;" v-model="selected" name="columns-selected" :options="columns">
                </b-form-checkbox-group>
            </b-form-group>
        </div>
        </div>
        <table-viewer :rows="rows" :columns="columns" :distributions="distributions" :encodings="encodings"></table-viewer>
    </div>
</template>
<script>
import TableViewer from './TableViewer'
import Papa from 'papaparse'
import sql from 'sql.js'
export default {
  name: 'Home',
  data () {
    return {
      rows: [],
      db: null,
      query: "",
      selected: [], // Must be an array reference!
      columns : ['WKR_ID',	'L1_KEY',	'DSG_CHG_ID',	'L1_DSG_CHG_PRT_NO',	'L1_BASIC_PRT_NO',
      'L1_DSG_CHG_NO',	'SUPLR_NO',	'BUYER_DESK_ID',	'BUYER_USER_ID',
      'BUYER_STG_ID',	'SUPLR_STG_ID',	'PRT_TPE',	'RFQ_NO',	'BASE_CURR',
      'DUE_DT',	'Q_CAL_FINAL_TOT_CS',	'QUOTE_NO',	'CURRNT_OWNER',
      'CURRENT_DESK_ID',	'CURRENT_USER_ID',	'WRK_STATUS',	'Q_BUYER_STATUS',
      'Q_SUPLR_STATUS',	'IS_TOOL_ATTCHD',	'VERSION',	'CREATED_USER_ID',
      'CREATED_DT_TS',	'UPDATED_USER_ID',	'UPDATED_DT_TS',	'WKR_LIST_COMMENT'],
      distributions: [],
      encodings: [],
    }
  },

  watch: {
      selected: function (val) {
        var query = "SELECT ";
        if (this.selected.length != this.columns.length) {
        this.selected.forEach((elem) =>{
          query += elem + ", ";
        })
        query = query.slice(0, -2);
        query += " FROM tableName";
        query = query.replace(/,\s*$/, "");
        this.executeQuery(query)
        };
      },
      rows: function (val) {
        this.createDistributions();
      }
    },
  components: {
    TableViewer
  },
  methods:{
    upload(e) {
        const that = this
        const fileToLoad = event.target.files[0]
        const reader = new FileReader()
        reader.onload = fileLoadedEvent => {
            Papa.parse(fileLoadedEvent.target.result, {
                header: true,
                dynamicTyping: true,

                complete(results) {
                    that.rows = results.data
                    that.createDB(results.data)
                },
                error(errors) {
                    console.log('error', errors)
                }
            })
        }
        reader.readAsText(fileToLoad)
    },

    createDistributions(){
        var newDistributions = [];
        console.log("gettings dists")
        var newEncodings = [];
        for (var i =0; i < this.columns.length; i++){

          var groupQuery = "SELECT " + this.columns[i] + ",  COUNT(*) FROM  tableName GROUP BY " + this.columns[i] + ";"
          var contents = this.db.exec(groupQuery);
          var rows = contents[0].values

          // for (var j = 0; j < rows.length; j++) {
            var vals = [
              {a: 'A', b: 28}, {a: 'B', b: 55}, {a: 'C', b: 43},
              {a: 'D', b: 91}, {a: 'E', b: 81}, {a: 'F', b: 53},
              {a: 'G', b: 19}, {a: 'H', b: 87}, {a: 'I', b: 52}
            ]

            var encoding = {
                x: {field: 'a', type: 'ordinal'},
                y: {field: 'b', type: 'quantitative'}
              }


          newDistributions.push(vals)
          newEncodings.push(encoding)


        }


        this.distributions = newDistributions;
        this.encodings = newEncodings;

    },

    createDB(data) {

        const columns  = this.columns;
        this.db = new sql.Database();
        var sqlstr = "CREATE TABLE tableName (";
        for (var i = 0; i < columns.length; i++) {

            var type = "varchar"

            if (i == columns.length - 1) {
                sqlstr += columns[i] + " " + type;
            } else {
                sqlstr += columns[i] + " " + type + ", ";
            }
        }
        sqlstr += ");";
        data.forEach((elem) => {
            sqlstr += "INSERT INTO tableName VALUES (";
            Object.keys(elem).forEach(key => {
                    sqlstr += "'" + String(elem[key]) + "'" + ", ";
            })
            sqlstr = sqlstr.slice(0, -2) + ");"
        })
        this.db.run(sqlstr);
    },

    executeQuery(query) {
        var contents = this.db.exec(query);
        var newData = [];
        var columns = contents[0].columns
        var rows = contents[0].values
        var newDataArr = []

        for (var j = 0; j < rows.length; j++) {

            var newData = {};
            for (var i = 0; i < columns.length; i++) {
                newData[columns[i]] = rows[j][i];
            }
            newDataArr.push(newData)
        }
        this.rows = newDataArr;
        this.columns  = columns;
    },
  },

  mounted: function () {

    // SELECT
  //  SUPLR_NO,
  //  COUNT(PRT_TYPE)
  // FROM
  //  tableName
  // GROUP BY
  //  SUPLR_NO;
    // SELECT SUPLR_NO COUNT(PRT_TYPE), FROM tableName GROUP BY SUPLR_NO
    // SELECT key1, key2, COUNT(id) FROM tableName GROUP BY key1, key2 ORDER BY key1, key2
    // SELECT key1, key2, COUNT(id) FROM tableName GROUP BY key1, key2 ORDER BY key1, key2
    for (var i = 0; i < this.columns.length; i++){
      this.selected.push(this.columns[i])
      }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.home{
  margin: 0 auto;
   text-align: center;
   padding: 2rem;
}

.column-picker{
  -webkit-appearance: none;
  .checkbox{
  display: inline;
}
}


</style>
