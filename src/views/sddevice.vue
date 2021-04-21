<template>
  <div id="Bclass">
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="设备名称">
        <el-input
          type="text"
          size="small"
          v-model="formInline.age2"
          placeholder="请输入设备名称"
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" size="small " @click="onSubmit" icon="el-icon-search">搜 索</el-button>
        <el-button @click="onCz" size="small " icon="el-icon-refresh">重 置</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      <el-col>
        <el-button type="primary" icon="el-icon-plus" @click="dialogVisible = true" size="mini">新增</el-button>
        <el-button type="success" icon="el-icon-minus" disabled size="mini">修改</el-button>
      </el-col>
    </el-row>
    <!-- 添加 -->
    <el-dialog title="添加设备信息" :visible.sync="dialogVisible">
      <el-form
        :model="numberValidateForm"
        ref="numberValidateForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="设备类型">
          <el-select v-model="numberValidateForm.de" prop="de" placeholder="进门或出门">
            <el-option
              v-for="(item, index) in dq2"
              :key="index"
              :label="item.dictLabel"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="设备IP地址" prop="ds">
          <el-input type="text" v-model="numberValidateForm.ds" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="识别失败提示语" prop="tl">
          <el-input type="text" v-model="numberValidateForm.tl" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="识别成功提示语" prop="am">
          <el-input type="text" v-model="numberValidateForm.am" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="设备号" prop="ne">
          <el-input type="text" v-model="numberValidateForm.ne" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="设备名称" prop="sn">
          <el-input type="text" v-model="numberValidateForm.sn" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="考勤规则" prop="ar">
          <el-select v-model="numberValidateForm.ar">
            <el-option
              v-for="(item, index) in dq3"
              :key="index"
              :label="item.attenceRuleName"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="Cha()">确 定</el-button>
      </span>
    </el-dialog>
    <el-row>
      <el-col class="Oinn" :span="24">
        <el-table ref="multipleTable" :data="tableData" tooltip-effect="dark" style="width: 100%">
          <el-table-column type="selection" width="55"></el-table-column>
          <el-table-column label="序号" width="100">
            <template slot-scope="scope">{{ scope.row.id }}</template>
          </el-table-column>
          <el-table-column label="设备IP地址" width="100">
            <template slot-scope="scope">{{scope.row.deviceIpAddress}}</template>
          </el-table-column>
          <el-table-column label="设备出入类型" prop="deviceAccessType"></el-table-column>
          <el-table-column label="设备号" prop="deviceNo"></el-table-column>
          <el-table-column label="设备名称" prop="deviceName"></el-table-column>
          <el-table-column label="考勤规则" prop="attenceRuleName"></el-table-column>
          <el-table-column prop="createTime" label="创建时间"></el-table-column>
          <el-table-column label="状态" width="90">
            <template>
              <el-switch v-model="value1"></el-switch>
            </template>
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button
                type="success"
                size="mini"
                icon="el-icon-edit"
                @click="Xiu(scope.row.id)"
                circle
              ></el-button>

            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
    <!-- 修改 -->
    <el-dialog title="修改设备信息" :visible.sync="dialogVisible2" width="width: 41%">
      <el-form
        :model="numberValidateForm"
        ref="numberValidateForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="设备类型">
          <el-select v-model="Cxun2.deviceAccessType" placeholder="进门或出门">
            <el-option
              v-for="(item, index) in dq2"
              :key="index"
              :label="item.dictLabel"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="设备IP地址" prop="age">
          <el-input type="text" v-model="Cxun2.deviceIpAddress" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="识别成功提示语" prop="age">
          <el-input type="text" v-model="Cxun2.tipsPairFail" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="设备号" prop="age">
          <el-input type="text" v-model="Cxun2.sn" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="设备名称" prop="age">
          <el-input type="text" v-model="Cxun2.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="考勤规则">
          <el-select v-model="Cxun2.attenceRuleName ">
            <el-option
              v-for="(item, index) in dq3"
              :key="index"
              :label="item.attenceRuleName"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible2 = false">取 消</el-button>
        <el-button type="primary" @click="Xiugai()">确 定</el-button>
      </span>
    </el-dialog>
     <div class="block">

      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage4"
        :page-sizes="[10, 20, 30, 40]"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </div>
  </div>
</template>
<script>
export default {
  name: "Bclass",
  data() {
    return {
       pageIndex:0,
      total:0,
      totalPage:0,
      currentPage4: 1,
      dialogVisible: false,
      dialogVisible2: false,
      numberValidateForm: {
        am: "",
        ar: "",
        de: "",
        ds: "",
        ne: "",
        sn: "",
        tl: ""
      },
      dq2: [],
      dq3: [],
      formInline: {
        age2: ""
      },
      value1: true,
      tableData: [],
      Cxun2: [],
      idd: "",
      Jchu: []
    };
  },
  created() {
    this.axios({
      method: "get",
      url:
        "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/device_access_type",
      headers: {
        Authorization: window.sessionStorage.token
      }
    }).then(relt => {
      this.Jchu = relt.data.data;
    });
    this.axios({
      method: "get",
      url:
        "http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=10",
      headers: {
        Authorization: window.sessionStorage.token
      }
    }).then(relt => {
      this.tableData = relt.data.data.list;
      this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
    });
    this.axios({
      method: "get",
      url:
        "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/device_access_type",
      headers: {
        Authorization: window.sessionStorage.token
      }
    }).then(relt => {
      this.dq2 = relt.data.data;
    });
    this.axios({
      method: "get",
      url:
        "http://122.112.253.28:8095/prod-api/smartdor/sddevice/getAttenceRuleList",
      headers: {
        Authorization: window.sessionStorage.token
      }
    }).then(relt => {
      this.dq3 = relt.data.data;
    });
    // this.axios({
    //   method: "get",
    //   url:
    //     "http://122.112.253.28:8095/prod-api/basedata/bdormitory/list?pageNum=1&pageSize=10&className=&status=&classTeacherName=",
    //   headers: {
    //     Authorization: window.sessionStorage.token
    //   }
    // }).then(relt => {
    //   this.tableData = relt.data.data.list;
    //   if (relt.data.data.list.status == 0) {
    //     this.value1 = false;
    //   } else {
    //     this.value1 = true;
    //   }
    // });
  },
  methods: {
    handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        // var that = this;
        this.axios({
          method: "get",
          url:
            `http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=${val}`,
          headers: {
            Authorization: window.sessionStorage.token
          }
        }).then(relt => {
          this.tableData = relt.data.data.list;
            this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
        });
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
        // this.pageIndex = val;
           this.axios({
          method: "get",
          url:
            `http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=${val}&pageSize=10`,
          headers: {
            Authorization: window.sessionStorage.token
          }
        }).then(relt => {
          this.tableData = relt.data.data.list;
            this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
        });
      },
    onSubmit() {
      this.axios({
        method: "get",
        url:
          "http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=10&deviceName=" +
          this.formInline.age2,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(relt => {
        this.tableData = relt.data.data.list;
          this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
      });
    },
    onCz() {
      (this.formInline.age = ""),
        (this.formInline.region = ""),
        (this.formInline.age3 = ""),
        (this.formInline.age2 = ""),
        this.axios({
          method: "get",
          url:
            "http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=10",
          headers: {
            Authorization: window.sessionStorage.token
          }
        }).then(relt => {
          this.tableData = relt.data.data.list;
            this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
        });
    },
    Cha() {
      this.axios({
        method: "POST",
        url: "http://122.112.253.28:8095/prod-api/smartdor/sddevice/create",
        data: {
          appWelcomeMsg: this.numberValidateForm.am,
          attenceRuleId: this.numberValidateForm.ar,
          deviceAccessType: "1",
          deviceIpAddress: this.numberValidateForm.ds,
          name: this.numberValidateForm.ne,
          sn: this.numberValidateForm.sn,
          tipsPairFail: this.numberValidateForm.tl
        },
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(relt => {
        console.log(relt);
        if (relt.data.code == 500) {
          this.$notify.error({
            title: "错误",
            message: "该信息存在"
          });
        } else {
          this.dialogVisible = false;
          this.$message({
            message: "新增成功!",
            type: "success"
          });
          this.numberValidateForm.am= "";
          this.numberValidateForm.ar = "";
          this.numberValidateForm.tl= "";
          this.numberValidateForm.sn= "";
          this.numberValidateForm.ne = "";
          this.numberValidateForm.ds= "";
          this.axios({
            method: "get",
            url:
              "http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=10",
            headers: {
              Authorization: window.sessionStorage.token
            }
          }).then(relt => {
            this.tableData = relt.data.data.list;
              this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
          });
        }
      });
    },
    Xiu(id) {
      this.dialogVisible2 = true;
      this.axios({
        method: "GET",
        url: "http://122.112.253.28:8095/prod-api/smartdor/sddevice/info/" + id,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(relt => {
        this.Cxun2 = relt.data.data;
        this.idd = relt.data.data.id;
      });
    },
    Xiugai() {
      this.axios({
        method: "PUT",
        url:
          "http://122.112.253.28:8095/prod-api/smartdor/sddevice/update/" +
          this.idd,
        headers: {
          Authorization: window.sessionStorage.token
        },
        data: this.Cxun2
      }).then(() => {
        this.$message({
          message: "修改成功!",
          type: "success"
        });
        this.dialogVisible2 = false;
        this.axios({
          method: "get",
          url:
            "http://122.112.253.28:8095/prod-api/smartdor/sddevice/list?pageNum=1&pageSize=10",
          headers: {
            Authorization: window.sessionStorage.token
          }
        }).then(relt => {
          this.tableData = relt.data.data.list;
            this.total = relt.data.data.total;
      this.totalPage = relt.data.data.totalPage;
        });
      });
    }
  }
};
</script>
<style >
.block{
  text-align: center;
}
.Oinn .el-table .cell {
  white-space: nowrap;
}
.Oinn .el-table th.is-leaf {
  background: #f8f8f9;
}
.Oinn .el-table td,
.Oinn .el-table th {
  text-align: center;
}
</style>
<style  scoped>
.demo-form-inline .el-select {
  margin-right: 50px;
}
.demo-ruleForm {
  display: flex;
  flex-flow: column;
}
.demo-ruleForm .el-form-item {
  flex: 1;
  font-size: 12px;
}
.Oinn {
  margin-top: 10px;
}
#Bclass {
  padding: 20px 0 0 20px;
}
.el-form-item {
  font-weight: bold;
}
</style>
