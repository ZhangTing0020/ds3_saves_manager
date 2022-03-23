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
    <el-dialog
      title="备份存档"
      :visible.sync="isShowSaveCard"
      width="60%"
    >
      <el-card>
        <el-form ref="card0" :model="form" :rules="rules" label-width="80px">
            <el-form-item prop="saveNameRule" label="存档名">
              <el-input v-model="form.saveName"></el-input>
            </el-form-item>
            <el-form-item label="存档位置">
              <el-input v-model="form.localSaveFilePath" :disabled="true" style="width: 100%;"></el-input>
            </el-form-item>
            <el-form-item label="备份位置">
              <el-row :gutter="10">
                <el-col :span="20">
                  <el-input v-model="form.savesMgrDirPath" :disabled="true" style="width: 100%;"></el-input>
                </el-col>
                <el-col :span="2">
                  <el-button @click="changeSaveDir()">更改</el-button>
                </el-col>
              </el-row>
            </el-form-item>
        </el-form>
      </el-card>
      <el-button
        @click="onSaveInfo()"
        class="submitButton"
        type="primary"
        >备份</el-button
      >
    </el-dialog>
  </div>
</template>

<script>
window.electron = require('electron');
const fs = require('fs');
const path = require('path');
const moment = require('moment');
export default {
  name: "HelloWorld",
  methods: {

    deleteRow(index, rows) {
      rows.splice(index, 1);
    },

    getGameSaveDirEnv() {
      const cmd = require('node-cmd');
      let cmdRes = cmd.runSync('echo %APPDATA%');
      let localSaveFilePath0 = cmdRes.data;
      if(cmdRes.err) {
        // 让玩家指定游戏目录
        localSaveFilePath0 = this.onOpenFilePositionButtonClick('黑暗之魂3存档');
      }
      if("" === cmdRes.data) {
        return ;
      }
      this.form.localSaveFilePath = path.join(localSaveFilePath0.replace(/\n/g, ""), 'DarkSoulsIII');
    },

    checkDirExists(title, filePath) {
      if("" === filePath) {
        // TODO
        // dialog warning when got home_env failed
        // 让玩家指定游戏目录
        filePath = this.onOpenFilePositionButtonClick(title);
        if("" === filePath) {
          return "";
        }
      }
      this.ensureDirSync(filePath);
      return filePath;
    },
    ensureDirSync(filePath) {
      if(!filePath) { return ; }
      if(!fs.existsSync(filePath)) {
        fs.mkdirSync(filePath);
        return ;
      }
    },

    createSavesFile() {
      const savesPath = path.join(process.cwd(), "saves");
      this.ensureDirSync(savesPath);
      this.form.savesMgrDirPath = savesPath;
    },

    on_click_save() {
      this.form.localSaveFilePath = this.checkDirExists('黑暗之魂3存档', this.form.localSaveFilePath);
      if(!this.form.localSaveFilePath) {
        // TODO
        // 让玩家选择存档位置
      } 
      this.form.savesMgrDirPath = this.checkDirExists('备份存档位置', this.form.savesMgrDirPath);
      if(!this.form.savesMgrDirPath) {
        // TODO
        // 让玩家选择存档位置
      } 

      this.isShowSaveCard = true;
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
      this.form.localSaveFilePath = this.checkDirExists('黑暗之魂3存档', this.form.localSaveFilePath);
      if(!this.form.localSaveFilePath) {
        // TODO
        // 让玩家选择存档位置
      } 
      this.form.savesMgrDirPath = this.checkDirExists('备份存档位置', this.form.savesMgrDirPath);
      if(!this.form.savesMgrDirPath) {
        // TODO
        // 让玩家选择存档位置
      } 



    },
    onSaveInfo() {
      if(!this.form.saveName || !this.form.localSaveFilePath || !this.form.savesMgrDirPath) { return ; }

      const toPath = path.join(this.form.savesMgrDirPath, this.form.saveName);
      const fromPath = this.form.localSaveFilePath;
      this.ensureDirSync(fromPath);
      if(!fs.existsSync(toPath)) {
        
        let selection = window.electron.remote.dialog.showMessageBoxSync({
            message : `存档名存在，是否覆盖存档？`,
            type : "info",
            buttons : ['确认', '取消']
        })

        if(1 === selection) { return ; }

        fs.rmdirSync(toPath);
      }

      this.ensureDirSync(toPath);
      fs.cpSync(fromPath, toPath);

      let nowTime = moment(Date()).format("YYYY-MM-DD HH:MM:SS");
      console.log("nowTime = ", nowTime);
      this.tableData.push({
          date: nowTime,
          save: this.form.saveName,
          steam_id: "bhfjdasu3",
          location: toPath,
        })

      this.isShowSaveCard = false;
      this.form.saveName = "";
    },
    onClickRestore() {

    },
    changeSaveDir() {
      //警告，建议不要改动存档位置，如果改动，不能保证程序能够识别存档位置
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
          date: "2016-05-03 19:00",
          save: "111",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
        {
          date: "2016-05-03 19:00",
          save: "222",
          steam_id: "bhfjdasu3",
          location: "D/abc",
        },
      ],
      isShowSaveCard: false,
      form: {
        saveName: "",
        localSaveFilePath: "",
        savesMgrDirPath: "",
      },
      rules: {
        saveNameRule: [
          { required: true, trigger: "blur" },
          // {
          //   pattern: /^[a-zA-Z0-9]+$/,
          //   message: "不允许输入中文",
          //   trigger: "blur",
          // },
        ],
      },
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