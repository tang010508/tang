<template>
  <div class="div_d">
    <el-dialog title="新增班级信息" :visible.sync="Xinz" width="30%">
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="班级名称" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="班主任" prop="region">
          <el-select v-model="ruleForm.region" placeholder="请选择">
            <el-option
              v-for="(item2, key2) in list2"
              :key="key2"
              :value="item2.id"
            >{{item2.username}}</el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="Xinz = false">取 消</el-button>
        <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="修改班级信息" :visible.sync="dialogFormVisible2">
      <el-form :model="form1">
        <el-form-item label="班级名称">
          <el-input v-model="form1.className" disabled autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="班主任">
          <el-select v-model="form1.classTeacherId">
            <el-option
              v-for="(item4, key4) in list4"
              :key="key4"
              :value="item4.id"
              :label="item4.username"
            >{{item4.username}}</el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="qx">取 消</el-button>
        <el-button type="primary" @click="onSubmit2">确 定</el-button>
      </div>
    </el-dialog>

    <div class="div_d1">
      <div class="demo-input-suffix div_d2">
        班级名称：
        <el-input placeholder="请输入班级名称" v-model="input1" clearable class="el_input_1"></el-input>
      </div>
      <div class="demo-input-suffix div_d2">
        班主任：
        <el-input placeholder="请输入班级班主任" v-model="input2" clearable class="el_input_1"></el-input>
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
        <el-table-column prop="className" label="班级名称"></el-table-column>
        <el-table-column prop="classTeacherName" label="班主任"></el-table-column>
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
            <el-button type="success" icon="el-icon-edit" circle @click="Xiug(scope.row.id)"></el-button>
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
      input1: "",
      input2: "",
      pageNum: 1,
      pageSize: 100,
      list: [],
      list2: [],
      list4: [],
      value: "",
      value1: "",
      jb: [],
      Xinz: false,
      dialogFormVisible2: false,
      multipleSelection: [],
      ruleForm: {
        name: "",
        region: ""
      },
      rules: {
        name: [
          { required: true, message: "班级名称不能为空", trigger: "blur" }
        ],
        region: [
          { required: true, message: "班主任不能为空", trigger: "change" }
        ]
      },
      form1: {
        className: "",
        classTeacherId: []
      }
    };
  },
  mounted() {
    this.xiala();
    this.xiala2();
    this.xians();
    this.banzr();
  },
  methods: {
    xiala() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_normal_disable",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          that.list = response.data.data;
        })
        .catch(error => {});
    },

    xiala2() {
      var that = this;
      this.axios({
        url: "http://122.112.253.28:8095/prod-api/sys/user/getClassTeacherList",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(response => {
        // console.log(response)
        that.list4 = response.data.data;
      });
    },

    xians() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/basedata/bclass/list?pageNum=1&pageSize=100",
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
          "http://122.112.253.28:8095/prod-api/basedata/bclass/deleteByIds/" +
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
          url: `http://122.112.253.28:8095/prod-api/basedata/bclass/list?pageNum=${that.pageNum}&pageSize=${that.pageSize}&className=${that.input1}&classTeacherName=${that.input2}&status=${that.value}`,
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          },
          data: {
            className: that.input1,
            classTeacherName: that.input2,
            status: that.value
          }
        })
        .then(response => {
          that.jb = response.data.data.list;
        });
    },

    Xiug(id) {
      this.dialogFormVisible2 = true;
      var that = this;
      that
        .axios({
          url: `http://122.112.253.28:8095/prod-api/basedata/bclass/${id}`,
          method: "GET",
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
          // console.log(response)
          that.form1 = response.data.data;
        });
    },

    onSubmit2() {
      var that = this;
      that
        .axios({
          url: `http://122.112.253.28:8095/prod-api/basedata/bclass/update/${that.form1.id}`,
          method: "PUT",
          data: that.form1,
          headers: {
            Authorization: window.sessionStorage.token
          }
        })
        .then(response => {
            that.form1 = response.data.data;
          if (response.data.code == "200") {
            that.$message({
              type: "success",
              message: "修改成功"
            });
            that.form1 = {
              className2: "",
              classTeacherId2: []
            };
            that.dialogFormVisible2 = false;
            that.xians();
          } else {
            that.$message({
              type: "info",
              message: "修改失败"
            });
          }
        })
        .catch(error => {
          console.log(error);
        });
    },

    qx() {
      this.dialogFormVisible2 = false;
    },

    submitForm(formName) {
      var that = this;
      that.$refs[formName].validate(valid => {
        that
          .axios({
            url: "http://122.112.253.28:8095/prod-api/basedata/bclass/create",
            method: "POST",
            data: {
              className: that.ruleForm.name,
              classTeacherId: that.ruleForm.region,
              status: "1"
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
  width: 280px;
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
