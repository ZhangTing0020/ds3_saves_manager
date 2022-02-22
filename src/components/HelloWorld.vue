<template>
  <div class="hello">
    <div class="top">
      <div class="left">
        <el-button type="primary" plain class="save">主要按钮</el-button>
      </div>
      <div class="right">
        <el-button type="primary" plain class="restore">主要按钮</el-button>
      </div>
    </div>

    <div class="table">
      <el-table
        :data="tableData"
        style="
           {
            width: 100%;
          }
        "
        height="100%"
      >
        <el-table-column fixed prop="date" label="日期"> </el-table-column>
        <el-table-column prop="save" label="存档"> </el-table-column>
        <el-table-column prop="steam_id" label="steam-id"> </el-table-column>
        <el-table-column prop="location" label="位置"> </el-table-column>
        <el-table-column fixed="right" label="操作">
          <template slot-scope="scope">
            <el-button
              @click.native.prevent="deleteRow(scope.$index, tableData)"
              type="text"
              size="small"
            >
              移除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  methods: {
    deleteRow(index, rows) {
      rows.splice(index, 1);
    },
  },
  data() {
    return {
      tableData: [
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
      ],
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.hello {
  width: 100vw;
}
.top {
  width: 100%;
  height: 25vh;
  // margin-bottom: 4vh;
  .left {
    display: inline-block;
    .save {
      width: 50vw;
      height: 30vh;

      font-size: 50px;
    }
  }
  .right {
    display: inline-block;
    .restore {
      width: 50vw;
      height: 30vh;
      font-size: 50px;
    }
  }
}
.table {
  height: 65vh;
  width: 100%;
  .el-table_2_column_6 {
    width: 15%;
  }
  .el-table_2_column_7 {
    width: 20%;
  }
  .el-table_2_column_8 {
    width: 15%;
  }
  .el-table_2_column_9 {
    width: 40%;
  }
  .el-table_2_column_10 {
    width: 10%;
  }
}
</style>