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

    getGameSaveDirEnv() {
      const cmd = require('node-cmd');
      const path = require('path');
      let localSaveFilePath = cmd.runSync('echo %APPDATA%');
      if(localSaveFilePath.err) {
        // TODO
        // dialog warning when got home_env failed
        // 让玩家指定游戏目录
        this.localSaveFilePath = this.onOpenFilePositionButtonClick('黑暗之魂3存档');
        return ;
      }
      this.localSaveFilePath = path.join(localSaveFilePath.data.replace(/\n/g, ""), 'DarkSoulsIII');
    },

    checkGameFileExists(title, filePath) {
      if("" === filePath) {
        // TODO
        // dialog warning when got home_env failed
        // 让玩家指定游戏目录
        filePath = this.onOpenFilePositionButtonClick(title);
        if("" === filePath) {
          return false;
        }
      }

      return fs.existsSync(filePath);
    },

    createSavesFile() {
      const savesPath = path.join(process.cwd(), "saves");
      if(!fs.existsSync(savesPath)) {
        fs.mkdirSync(savesPath)
      }
      this.savesMgrDirPath = savesPath;
    },

    on_click_save() {
      // if(!this.checkGameFileExists('黑暗之魂3存档', this.localSaveFilePath)) {
      //   return ;
      // }
      if(!this.checkGameFileExists('导出存档', this.savesMgrDirPath)) {
        this.createSavesFile();
        if(!this.checkGameFileExists('导出存档', this.savesMgrDirPath)) {
          return ;
        }
      }
      
      //TODO 
      // 从输入框中收集存档信息，比如存档名
      const savesPath = path.join(this.savesMgrDirPath, "111");
      if(!this.checkVaildExportSavesDir(savesPath)) {
        // TODO 
        // 警告：该存档已存在，请重新取名
        window.electron.remote.dialog.showMessageBoxSync({
            message : `存档名存在，请重新取名`,
            type : "info",
            buttons : ['确认']
        })
        return ;
      }


    },


    onOpenFilePositionButtonClick(title) {
      let filePath = "";
      window.electron.remote.dialog.showOpenDialog({
          title: `请选择${title}路径`,
          filters: [
              { name: 'Custom File Type', extensions: ['js', 'html', 'json'] },
            ],
          properties : ['openDirectory']
      }).then(result => {
          console.log(result.canceled)
          console.log(result.filePaths)
          if(!result.canceled) {
            filePath = result.filePaths[0];
          }
      }).catch(err => {
        console.log(err)
      });

      return filePath;
    },

    on_click_restore() {
      if(this.checkGameFileExists()) {
        return ;
      }


    },
    checkVaildExportSavesDir(dirName) {
      if(fs.existsSync(dirName)) {
        return false;
      }

      fs.mkdirSync(dirName);
      return true;
    },
  },


  created() {
    this.getGameSaveDirEnv();
    this.createSavesFile();
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
      localSaveFilePath: "",
      savesMgrDirPath: "",
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