<template>
  <div class="home">
    <h1>Please upload a CSV</h1>
    <div style="margin-top: 3rem;"></div>

    <input
      id="fileInput"
      class="input"
      type="file"
      @change="upload">

    <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Enter SQL Query" aria-label="SQL Query" @change="executeQuery" aria-describedby="basic-addon2">
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button">Enter Query</button>
        </div>
      </div>
      <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Enter slope" aria-label="SQL Query" @change="executeQuery" aria-describedby="basic-addon2">
          <div class="input-group-append">
            <button class="btn btn-outline-secondary" type="button">Enter the correct slope</button>
          </div>
        </div>
    <table-viewer :rows="rows" :columns="columns"></table-viewer>
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
      columns : ['WKR_ID',	'L1_KEY',	'DSG_CHG_ID',	'L1_DSG_CHG_PRT_NO',	'L1_BASIC_PRT_NO',
      'L1_DSG_CHG_NO',	'SUPLR_NO',	'BUYER_DESK_ID',	'BUYER_USER_ID',
      'BUYER_STG_ID',	'SUPLR_STG_ID',	'PRT_TPE',	'RFQ_NO',	'BASE_CURR',
      'DUE_DT',	'Q_CAL_FINAL_TOT_CS',	'QUOTE_NO',	'CURRNT_OWNER',
      'CURRENT_DESK_ID',	'CURRENT_USER_ID',	'WRK_STATUS',	'Q_BUYER_STATUS',
      'Q_SUPLR_STATUS',	'IS_TOOL_ATTCHD',	'VERSION',	'CREATED_USER_ID',
      'CREATED_DT_TS',	'UPDATED_USER_ID',	'UPDATED_DT_TS',	'WKR_LIST_COMMENT']
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

    executeQuery(e) {


        var query = event.target.value

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
        console.log(newDataArr)
        this.rows = newDataArr;
        this.columns  = columns;
        console.log("new cols ", columns)

        // this.setColumnInformation(newDataArr[0])

    },
  },

  mounted: function () {

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


</style>
