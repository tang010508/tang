<template>
  <div>
    <el-row>
      <el-col :span="15" class="Datecol">
        <template>
          <div class="block">
            <span class="demonstration">日期</span>
            <el-date-picker v-model="value1" type="date" placeholder="选择日期"></el-date-picker>
          </div>
        </template>
      </el-col>
      <el-col :span="2">
        <el-button class="ssbt" type="primary" @click="select" size="mini">搜索</el-button>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24" class="tabcol">
        <template>
          <Row>
            <i-col span="24">
              <Table border :columns="columns1" :data="data1"></Table>

              <div style="margin-top: 20px;height: 80px">
                <Page :total="total" show-sizer show-elevator @on-change="change" />
              </div>
            </i-col>
          </Row>
        </template>
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
<script>
let url = "http://122.112.253.28:8095/prod-api";
import moment from "moment";
export default {
  component: {
    moment
  },
  data() {
    return {
      value1: "",
      total: "",
      studentNo: [],
      totalPage: 0,
      dialogVisible: false,
      total: 27,
      pageSize: 10,
      DSelectData: {},
      pageNum: 1,
      totalPage: 3,
      columns1: [
        {
          type: "selection",
          width: 60,
          align: "center"
        },
        {
          title: "序号",
          key: "id",
          width: 80,
          align: "center"
        },
        {
          title: "学号",
          align: "center",
          key: "studentNo",
          width: 150
        },
        {
          title: "班级",
          align: "center",
          key: "className",
          width: 140
        },
        {
          title: "学生姓名",
          align: "center",
          key: "studentName",
          width: 180
        },
        {
          title: "宿舍信息",
          align: "center",
          key: "dorAddress",
          width: 170
        },
        {
          title: "考勤时间",
          align: "center",
          key: "accessDate",
          width: 170
        },
        {
          title: "考勤状态",
          align: "center",
          key: "attenceStatus",
          width: 100
        },
        {
          title: "创建时间",
          align: "center",
          key: "createTime",
          width: 170
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
                      var that = this;
                      this.dialogVisible = true;
                      this.userID = params.row.id;
                      this.studentNo = params.row.studentNo;
                      this.accessDate = params.row.accessDate;
                      this.DanSelect(this.studentNo, this.accessDate);
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
    this.all();
  },
  methods: {
    handleClose() {},
    //查全部
    all() {
      this.axios({
        method: "GET",
        url: url + "/smartdor/sdAttence/list?pageNum=1&pageSize=10",
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(alldata => {
        console.log(alldata);
        this.data1 = alldata.data.data.list;
      });
    },
    //分页
    change(pageIndex) {
      var that = this;
      this.axios({
        method: "get",
        url:
          url +
          `/smartdor/sdAttence/list?pageNum=${pageIndex}&pageSize=${that.pageSize}`,
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
    //查询
    select() {
      if (this.value1) {
        this.value1 = moment(new Date(this.value1)).format("YYYY-MM-DD");
      } else {
        this.value1 = "";
      }
      this.axios({
        method: "GET",
        url:
          url +
          `/smartdor/sdAttence/list?pageNum=1&pageSize=10&checkDate=${this.value1}`,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(selectData => {
        console.log(selectData);
        this.data1 = selectData.data.data.list;
      });
    },
    //删除
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
            url: url + "/smartdor/sdAttence/deleteByIds/" + id,
            headers: {
              Authorization: window.sessionStorage.token
            }
          }).then(delDate => {
            this.all();
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    //查单个信息
    DanSelect(studentNo, accessDate) {
      console.log(studentNo);
      this.axios({
        method: "GET",
        url:
          url +
          `/smartdor/sdAttence/queryleave?studentNo=${studentNo}&leaveDate=${accessDate}`,
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(DanSelectData => {
        console.log(DanSelectData);
        var DanData = DanSelectData.data.data;
        if (DanData) {
          this.DSelectData = DanSelectData.data.data;
        }else{
            this.dialogVisible = false;
             this.$message({
            type: "warning",
            message: "宿管老师未处理此数据!!!"
          });

        }
      });
    }
  }
};
</script>
<style scoped>
.Datecol {
  font-weight: bold;
  margin-left: 20px;
  margin-top: 20px;
}
.ssbt {
  margin-left: -560px;
  margin-top: 25px;
}
.tabcol {
  margin-top: 15px;
}
.student{
    font-weight: bold;
    margin-bottom: 10px;
}
.student span{
    font-weight: 100
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
