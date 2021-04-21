<template>
  <div>
    <el-row class="mDd">
      <el-col :span="6" :offset="2">
        <template>
          学号
          <el-input class="niq" v-model="input" placeholder="请输入班级名称"></el-input>
        </template>
      </el-col>
      <el-col :span="6" :offset="2">
        <template>
          学生手机号
          <el-input class="niq" v-model="input1" placeholder="请输入班主任名称"></el-input>
        </template>
      </el-col>
      <el-col :span="6" :offset="2">
        <template>
          请假类型
          <el-select v-model="value" placeholder="班级状态">
            <el-option
              v-for="item in options"
              :key="item.dictValue"
              :label="item.dictLabel"
              :value="item.dictValue"
            ></el-option>
          </el-select>
        </template>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6" :offset="2">
        <template>
          状态
          <el-select v-model="value2" placeholder="班级状态">
            <el-option
              v-for="iteem in options2"
              :key="iteem.dictValue"
              :label="iteem.dictLabel"
              :value="iteem.dictValue"
            ></el-option>
          </el-select>
        </template>
      </el-col>
      <el-col :span="2" :offset="1">
        <el-button
          style="margin-top:10px;margin-left-50px;"
          type="primary"
          @click="select"
          size="mini"
        >查询</el-button>
      </el-col>
      <el-col :span="2" :offset="1">
        <el-button style="margin-top:10px;" type="primary" plain size="mini">重置</el-button>
      </el-col>
    </el-row>
    <el-row style="margin-top:20px;">
      <el-col :span="24">
        <Row>
          <i-col span="24">
            <Table border :columns="columns1" :data="data1"></Table>
            <div style="margin-top: 20px;height: 80px">
              <Page :total="total" show-sizer show-elevator @on-change="change" />
            </div>
          </i-col>
        </Row>
      </el-col>
    </el-row>
    <el-dialog
      title="请假详细"
      class="dialogel"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose"
    >
      <el-row>
        <el-col :span="12" class="student">
          学号：
          <span>{{ DSelectData.studentNo }}</span>
        </el-col>
        <el-col :span="12" class="student">
          班级：
          <span>{{ DSelectData.className }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12" class="student">
          学生姓名：
          <span>{{ DSelectData.studentName }}</span>
        </el-col>
        <el-col :span="12" class="student">
          学生手机号：
          <span>{{ DSelectData.studentPhone }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12" class="student">
          请假类型：
          <span>{{ DSelectData.leaveType }}</span>
        </el-col>
        <el-col :span="12" class="student">
          请假日期：
          <span>{{ DSelectData.leaveDate }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24" class="student">
          状态：
          <span>{{ DSelectData.status }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24" class="student">
          请假原因：
          <span>{{ DSelectData.leaveReason }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24" class="student">
          家长描述：
          <span>{{ DSelectData.parentDesc }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24" class="student">
          班主任描述：
          <span>{{ DSelectData.teacherDesc }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24" class="student">
          请假记录提交时间：
          <span>{{ DSelectData.createTime }}</span>
        </el-col>
      </el-row>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">关 闭</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<style scoped>
.mDd,
.demonstration {
  text-align: left;
  font-size: 14px;
  font-weight: bold;
  color: #606266;
  margin-top: 10px;
}
.niq {
  width: 70%;
}
</style>
<style>
.ivu-btn-circle {
  height: 30px !important;
  width: 30px !important;
  text-align: center !important;
  padding-left: 7px !important;
  border-radius: 50% !important;
}
</style>
<script>
let url = "http://122.112.253.28:8095/prod-api";
export default {
  data() {
    return {
      input: "",
      input1: "",
      options: [],
      total: 0,
      DSelectData: {},
      dialogVisible:false,
      pageNum: 1,
      totalPage: 3,
      data1: [],
      options2: [],
      value2: "",
      value: "",
      columns1: [
        {
          type: "selection",
          width: 60,
          align: "center"
        },
        {
          title: "序号",
          key: "id",
          width: 70,
          align: "center"
        },
        {
          title: "学号",
          key: "studentNo",
          align: "center",
          width: 120
        },
        {
          title: "班级",
          align: "center",
          key: "className",
          width: 120
        },
        {
          title: "学生姓名",
          align: "center",
          key: "studentName",
          width: 100
        },
        {
          title: "学生手机号",
          align: "center",
          key: "studentPhone",
          width: 180
        },
        {
          title: "请假类型",
          align: "center",
          key: "leaveType",
          width: 100
        },
        {
          title: "请假日期",
          align: "center",
          key: "leaveDate",
          width: 180
        },
        {
          title: "状态",
          align: "center",
          key: "status",
          width: 100
        },
        {
          title: "创建时间",
          align: "center",
          key: "createTime",
          width: 180
        },
        {
          title: "操作",
          key: "uses",
          align: "center",
          width: 200,
          render: (h, params) => {
            return h("div", [
              h(
                "i-button",
                {
                  props: {
                    type: "success",
                    icon: "ios-eye",
                    shape: "circle",
                    size: "small"
                  },
                  style: {
                    marginLeft: "20px"
                  },
                  on: {
                    click: () => {
                      ///basedata/bclass/150
                      var that = this;
                      this.axios({
                        url:
                          "http://122.112.253.28:8095/prod-api/basedata/bclass/" +
                          that.userID,
                        method: "GET",
                        headers: {
                          Authorization: window.sessionStorage.token
                        }
                      }).then(response => {
                        that.updateForm = response.data.data;
                      });
                      this.dialogVisible1 = true;
                      this.userID = params.row.id;
                      this.studentNo = params.row.studentNo;
                      this.createTime = params.row.createTime;
                      this.DanSelect(this.studentNo, this.createTime);
                    }
                  }
                },
                ""
              ),
              h(
                "Button",
                {
                  props: {
                    type: "error",
                    icon: "ios-trash",
                    shape: "circle",
                    size: "small"
                  },
                  style: {
                    marginLeft: "20px"
                  },
                  on: {
                    click: () => {
                      this.delClass(params.row.id);
                    }
                  }
                },
                ""
              )
            ]);
          }
        }
      ],
      data1: [],
      forms: {
        bumen: "",
        jibie: "",
        keshi: "",
        xiags: "",
        type: "",
        date: "",
        search: ""
      }
    };
  },
  mounted() {
    this.Ztai();
    this.leavestatus();
    this.All();
  },
  methods: {
    select() {},
    Ztai() {
      this.axios({
        method: "GET",
        url: url + "/sys/dict/detail/dictType/leave_type",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(leavetype => {
        this.options = leavetype.data.data;
      });
    },
    leavestatus() {
      this.axios({
        method: "GET",
        url: url + "/sys/dict/detail/dictType/leave_status",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(leavetype => {
        this.options2 = leavetype.data.data;
      });
    },
    All() {
      this.axios({
        method: "GET",
        url: url + "/smartdor/sdleave/list?pageNum=1&pageSize=10",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(allData => {
        this.data1 = allData.data.data.list;
      });
    },
    change(pageIndex) {
      var that = this;
      this.axios({
        method: "get",
        url:
          url +
          `/smartdor/sdleave/list?pageNum=${pageIndex}&pageSize=${that.pageSize}`,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(replace => {
        console.log(replace);
        this.data1 = replace.data.data.list;
        that.total = replace.data.data.total;
        that.totalPage = replace.data.data.totalPage;
      });
    },
    select() {
      this.axios({
        method: "GET",
        url:
          url +
          `/smartdor/sdleave/list?pageNum=1&pageSize=10&studentNo=${this.input}&leaveType=${this.value}&status=${this.value2}&studentPhone=${this.input1}`,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(selectData => {
        this.data1 = selectData.data.data.list;
      });
    },
    delClass(id) {
      this.$confirm(
        "是否确认删除考勤信息编号为'" + id + "'的数据项?",
        "删除提示",
        {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        }
      )
        .then(() => {
          this.$message({
            type: "success",
            message: "删除成功!"
          });
          this.axios({
            method: "DELETE",
            url: url + "/smartdor/sdleave/deleteByIds/" + id,
            headers: {
              Authorization: window.sessionStorage.token
            }
          }).then(delDate => {
            this.All();
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    DanSelect(studentNo, createTime) {
      console.log(studentNo);
      this.axios({
        method: "GET",
        url:
          url +
          `/smartdor/sdAttence/queryleave?studentNo=${studentNo}&leaveDate=${createTime}`,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(DanSelectData => {
        console.log(DanSelectData);
        var DanData = DanSelectData.data.data;
        if (DanData) {
          this.DSelectData = DanSelectData.data.data;
        } else {
          this.dialogVisible = false;
          this.$message({
            type: "warning",
            message: "老师未处理此数据!!!"
          });
        }
      });
    },
    handleClose(){
    }
  }
};
</script>
