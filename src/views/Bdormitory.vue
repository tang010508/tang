<template>
  <div class="div_d">
    <!-- 添加 -->
    <el-dialog title="宿舍添加信息" :visible.sync="Xinz" width="30%">
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="宿舍栋号">
          <el-select v-model="ruleForm.buildingNo" placeholder="宿舍栋号">
            <el-option
              v-for="(item5, key5) in nb"
              :key="key5"
              :value="item5.dictSort"
            >{{item5.dictLabel}}</el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="楼层">
          <el-select v-model="ruleForm.storey" placeholder="楼层">
            <el-option
              v-for="(item6, key6) in nb2"
              :key="key6"
              :value="item6.dictSort"
            >{{item6.dictLabel}}</el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="宿舍号">
          <el-input v-model="ruleForm.dormitoryNo"></el-input>
        </el-form-item>

        <el-form-item label="宿管老师">
          <el-select v-model="ruleForm.dormitoryTeacherId" placeholder="请选择">
            <el-option v-for="(item7, key7) in nb3" :key="key7" :value="item7.id">{{item7.username}}</el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="Xinz = false">取 消</el-button>
        <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
      </span>
    </el-dialog>

    <!-- 修改 -->
    <el-dialog title="修改宿舍信息" :visible.sync="dialogFormVisible2">
      <el-form ref="from2" :model="from2" label-width="80px">
        <el-form-item label="宿舍栋号">
          <el-select v-model="from2.buildingNo" placeholder="请选择" style="width:350px;" disabled>
            <el-option
              v-for="(item, key) in list2"
              :key="key"
              :value="item.buildingNo"
              :label="item.buildingNo"
            >{{ item.buildingNo }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="楼层">
          <el-select v-model="from2.storey" placeholder="请选择" style="width:350px;" disabled>
            <el-option
              v-for="(item, key) in list2"
              :key="key"
              :value="item.storey"
              :label="item.storey"
            >{{ item.storey }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="宿舍号">
          <el-select v-model="from2.dormitoryNo" placeholder="请选择" style="width:350px;" disabled>
            <el-option
              v-for="(item, key) in list2"
              :key="key"
              :value="item.dormitoryNo"
              :label="item.dormitoryNo"
            >{{ item.dormitoryNo }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="宿管老师">
          <el-select v-model="from2.dormitoryTeacherId" placeholder="请选择" style="width:350px;">
            <el-option
              v-for="(item8, key8) in list8"
              :key="key8"
              :value="item8.id"
              :label="item8.dormitoryTeacherName"
            >{{ item8.username }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit2">确定</el-button>
          <el-button @click="qx">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <div class="div_d1">
      <div class="demo-input-suffix div_d2">
        宿舍栋号：
        <el-select v-model="buildingNo" placeholder="请输入宿舍栋号">
          <el-option
            v-for="(item3,key3) in list3"
            :key="key3"
            :value="item3.dictLabel"
          >{{item3.dictLabel}}</el-option>
        </el-select>
      </div>
      <div class="demo-input-suffix div_d2">
        楼层：
        <el-select v-model="storey" placeholder="请输入楼层">
          <el-option
            v-for="(item4,key4) in list4"
            :key="key4"
            :value="item4.dictLabel"
          >{{item4.dictLabel}}</el-option>
        </el-select>
      </div>
      <div class="demo-input-suffix div_d2">
        宿舍号：
        <el-input placeholder="请输入宿舍号" v-model="dormitoryNo" clearable class="el_input_1"></el-input>
      </div>
      <div class="demo-input-suffix div_d2">
        状态：
        <el-select v-model="value" placeholder="班级状态">
          <el-option
            v-for="(item,key) in list"
            :key="key"
            :value="item.dictLabel"
          >{{item.dictLabel}}</el-option>
        </el-select>
      </div>
      <el-button type="primary" icon="el-icon-search" @click="Chaxun">查询</el-button>
      <el-button plain icon="el-icon-refresh">重置</el-button>
    </div>

    <el-row class="el_row_1">
      <el-button type="primary" icon="el-icon-plus" @click="Xinz = true">新增</el-button>
      <el-button type="success" icon="el-icon-edit">修改</el-button>
      <el-button type="danger" icon="el-icon-delete">删除</el-button>
    </el-row>

    <div class="div_d3">
      <el-table
        ref="multipleTable"
        :data="jb"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange"
      >
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column label="序号" width="120">
          <template slot-scope="scope">{{ scope.row.id }}</template>
        </el-table-column>
        <el-table-column prop="buildingNo" label="宿舍栋号"></el-table-column>
        <el-table-column prop="storey" label="楼层"></el-table-column>
        <el-table-column prop="dormitoryNo" label="宿舍号"></el-table-column>
        <el-table-column prop="dormitoryTeacherName" label="宿管老师"></el-table-column>
        <el-table-column prop="createTime" label="创建时间"></el-table-column>
        <el-table-column prop="modifyTime" label="修改时间"></el-table-column>
        <el-table-column prop="status" label="状态">
          <template slot-scope="scope">
            <div>
              <div style="position:relative;z-index:999">
                <el-switch
                  v-model="scope.row.status"
                  active-color="#13ce66"
                  inactive-color="#ff4949"
                ></el-switch>
              </div>
            </div>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button type="success" icon="el-icon-edit" circle @click="update(scope.row.id)"></el-button>
            <el-button type="danger" icon="el-icon-delete" circle @click="Delete(scope.row.id)"></el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buildingNo: "",
      storey: "",
      dormitoryNo: "",
      pageNum: 1,
      pageSize: 100,
      value: "",
      value1: "",
      value2: "",
      value3: "",
      jb: [],
      Xinz: false,
      multipleSelection: [],
      ruleForm: {
        name: "",
        region: ""
      },
      list: [],
      list2: [],
      list3: [],
      list4: [],
      list8: [],
      rules: {
        name: [
          { required: true, message: "班级名称不能为空", trigger: "blur" }
        ],
        region: [
          { required: true, message: "班主任不能为空", trigger: "change" }
        ]
      },
      nb: [],
      nb2: [],
      nb3: [],
      from2: {
        buildingNo: [],
        storey: [],
        dormitoryTeacherId: [],
        dormitoryNo: ""
      },
      dialogFormVisible2:false

    };
  },
  mounted() {
    this.xiala();
    this.xiala2();
    this.xiala3();
    this.xiala4();
    this.xiala5();
    this.xiala6();
    this.xiala7();
    this.xians();
    this.banzr();
  },
  methods: {
    xiala() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_building",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          // console.log(response.data)
          that.list3 = response.data.data;
        })
        .catch(error => {});
    },

    xiala2() {
      var that = this;
      that
        .axios({
          url:
            "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_storey",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
          that.list4 = response.data.data;
        });
    },

    xiala3() {
      var that = this;
      that
        .axios({
          url:
            "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_normal_disable",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
          that.list = response.data.data;
        });
    },

    xiala4() {
      var that = this;
      that
        .axios({
          url:
            "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_building",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
          that.nb = response.data.data;
        });
    },

    xiala5() {
      var that = this;
      that
        .axios({
          url:
            "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_storey",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(responer => {
          that.nb2 = responer.data.data;
        });
    },

    xiala6() {
      var that = this;
      that
        .axios({
          url: "http://122.112.253.28:8095/prod-api/sys/user/getDorTeacherList",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(responer => {
          that.nb3 = responer.data.data;
        });
    },

     xiala7() {
      var that = this;
      this.axios({
        url: "http://122.112.253.28:8095/prod-api/sys/user/getDorTeacherList",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          console.log(response.data);
          that.list8 = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },

    xians() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/basedata/bdormitory/list?pageNum=1&pageSize=10",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          that.jb = response.data.data.list;
        })
        .catch(error => {});
    },

    Delete(id) {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/basedata/bdormitory/deleteByIds/" +
          id,
        method: "DELETE",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(response => {
        // this.$confirm("是否确认删除班级编号为的数据项?", "提示", {
        //   confirmButtonText: "确定",
        //   cancelButtonText: "取消",
        //   type: "warning"
        // });
        // console.log(response)
        if (response.data.code == "200") {
          this.$message({
            type: "success",
            message: "删除成功!"
          });
          this.xians();
          // window.location.reload();
          // return;
        } else {
          this.$message({
            type: "info",
            message: "删除失败"
          });
        }
      });
    },

    banzr() {
      var that = this;
      that
        .axios({
          url:
            "http://122.112.253.28:8095/prod-api/sys/user/getClassTeacherList",
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
          that.list2 = response.data.data;
        });
    },

    Chaxun() {
      var that = this;
      that
        .axios({
          url: `http://122.112.253.28:8095/prod-api/basedata/bdormitory/list?pageNum=1&pageSize=10&buildingNo=${that.buildingNo}&storey=${that.storey}&dormitoryNo=${that.dormitoryNo}`,
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          },
          data: {
            buildingNo: that.buildingNo,
            storey: that.storey,
            dormitoryNo: that.dormitoryNo
          }
        })
        .then(response => {
          that.jb = response.data.data.list;
        });
    },

    submitForm(formName) {
      var that = this;
      that.$refs[formName].validate(valid => {
        that
          .axios({
            url:
              "http://122.112.253.28:8095/prod-api/basedata/bdormitory/create",
            method: "POST",
            data: {
              buildingNo: that.ruleForm.buildingNo,
              storey: that.ruleForm.storey,
              dormitoryNo: that.ruleForm.dormitoryNo,
              dormitoryTeacherId: that.ruleForm.dormitoryTeacherId
            },
            headers: {
              Authorization: window.sessionStorage.token
            }
          })
          .then(response => {
            if (response.data.code == "200") {
              that.$message({
                type: "success",
                message: "添加成功"
              });
              that.Xinz = false;
              this.xians();
            } else {
              that.$message({
                type: "info",
                message: "添加失败"
              });
            }
          });
      });
    },

    update(id) {
      // console.log(id)
      this.dialogFormVisible2 = true;
      var that = this;
      this.axios({
        url: `http://122.112.253.28:8095/prod-api/basedata/bdormitory/${id}`,
        method: "GET",

        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          this.from2 = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    // 修改提交
    onSubmit2() {
      var that = this;
      this.axios({
        url: `http://122.112.253.28:8095/prod-api/basedata/bdormitory/update/${this.from2.id}`,
        method: "PUT",
        data: that.from2,
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          this.from2 = response.data.data;
          if (response.data.code == "200") {
            this.$message({
              type: "success",
              message: "修改成功"
            });
            this.from2 = {
              name2: "",
              region2: []
            };
            this.dialogFormVisible2 = false;
            this.xians();
          } else {
            this.$message({
              type: "info",
              message: "修改失败"
            });
          }
        })
        .catch(error => {
          console.log(error);
        });
    },

    qx(){
      this.dialogFormVisible2 = false
    },

    resetForm(formName) {
      this.$refs[formName].resetFields();
    },

    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },

    handleSelectionChange(val) {
      this.multipleSelection = val;
    }
  }
};
</script>

 <style scoped>
.div_d {
  width: 1641px;
  height: 848px;
}
.el_input_1 {
  width: 180px;
  height: 32px;
}
.div_d1 {
  height: 65px;
}
.div_d2 {
  width: 312px;
  height: 58px;
  float: left;
}
.el_row_1 {
  height: 65px;
}
.div_d3 {
  width: 1641px;
}
</style>
