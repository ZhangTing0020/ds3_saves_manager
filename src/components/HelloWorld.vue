<template>
  <div class="hello">
    <div class="top">
      <div class="left">
        <el-button type="primary" class="save" @click='on_click_save'>备份存档</el-button>
      </div>
      <div class="right">
        <el-button type="primary" class="restore" @click='on_click_restore'>还原存档</el-button>
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
window.electron = require('electron');
const fs = require('fs');
const path = require('path');
export default {
  name: "HelloWorld",
  methods: {

    deleteRow(index, rows) {
      rows.splice(index, 1);
    },

    getHomeDirEnv() {
      const cmd = require('node-cmd');
      const path = require('path');
      let homeEnv = cmd.runSync('echo %APPDATA%');
      if(homeEnv.err) {
        // TODO
        // dialog warning when got home_env failed
        // 让玩家指定游戏目录
      }
      this.homeEnv = path.join(homeEnv.data.replace(/\n/g, ""), 'DarkSoulsIII');
    },

    checkGameFileExists() {
      if("" === this.homeEnv) {
        // TODO
        // dialog warning when got home_env failed
        // 让玩家指定游戏目录
      }

      return fs.existsSync(this.homeEnv);
    },

    createSavesFile() {
      const savesPath = path.join(__dirname, "saves");
      if(fs.existsSync(savesPath)) {
        fs.mkdirSync(savesPath)
      }
    },

    on_click_save() {
      // if(this.checkGameFileExists()) {
      //   return ;
      // }
      // require('@electron/remote/main').initialize();
      // console.log('electron = ', window.electron);
      this.onOpenFilePositionButtonClick();
    },


    onOpenFilePositionButtonClick() {
      window.electron.remote.dialog.showOpenDialog({
          title: "请选择游戏路径",
          filters: [
              { name: 'Custom File Type', extensions: ['js', 'html', 'json'] },
            ],
          properties : ['openDirectory']
      }).then(result => {
          console.log(result.canceled)
          console.log(result.filePaths)
      }).catch(err => {
        console.log(err)
      });
    },

    on_click_restore() {
      if(this.checkGameFileExists()) {
        return ;
      }


    }
  },

  created() {
    this.getHomeDirEnv();
    this.createSavesFile();
    this.checkGameFileExists();
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
      ],
      homeEnv: "",
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