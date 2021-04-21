<template>
  <div>
    <el-row class="zhengqi">
      <el-col :span="10" :offset="7">
        <div class="block">
          <span class="demonstration">卫生检查日期</span>
          <el-date-picker v-model="value1" type="date" placeholder="选择日期" size="mini"></el-date-picker>
        </div>
      </el-col>
      <el-col :span="2" :offset="1">
        <el-button type="primary" size="mini">查询</el-button>
      </el-col>
      <el-row class="zhengqi">
        <el-col :span="11" :offset="7">
          <el-button type="primary" @click=" add(), dialogVisible = true" size="mini">新增</el-button>
          <el-button type="success" disabled size="mini">修改</el-button>
          <el-button type="danger" disabled size="mini">删除</el-button>
        </el-col>
      </el-row>
    </el-row>
    <el-row>
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
  </div>
</template>
<script>
let url = "http://122.112.253.28:8095/prod-api";
export default {
  data() {
    return {
     pickerOptions: {
          disabledDate(time) {
            return time.getTime() > Date.now();
          },
      },
      value1: "",
      pageSize: 10,
      total: 27,
      totalPage: 3,
      totalPage: 0,
      columns1: [
        {
          type: "selection",
          width: 60,
          align: "center"
        },
        {
          title: "序号",
          key: "id",
          width: 100,
          align: "center"
        },
        {
          title: "卫生检查日期",
          key: "checkDate",
          align: "center",
          width: 121
        },
        {
          title: "宿舍栋号",
          align: "center",
          key: "buildingNo",
          width: 100
        },
        {
          title: "楼层",
          align: "center",
          key: "storey",
          width: 80
        },
        {
          title: "宿舍号",
          align: "center",
          key: "dormitoryNo",
          width: 80
        },
        {
          title: "卫生扣分项",
          align: "center",
          key: "deductIds",
          width: 170
        },
        {
          title: "总扣分",
          align: "center",
          key: "totalPdeduct",
          width: 80
        },
        {
          title: "总得分",
          align: "center",
          key: "totalScore",
          width: 100
        },
        {
          title: "创建时间",
          align: "center",
          key: "createTime",
          width: 168
        },
        {
          title: "操作",
          key: "uses",
          align: "center",
          width: 220,
          render: (h, params) => {
            return h("div", [
              h(
                "i-button",
                {
                  props: {
                    type: "success",
                    icon: "ios-create-outline",
                    shape: "circle",
                    size: "small"
                  },
                  style: {
                    marginLeft: "20px"
                  },
                  on: {
                    click: () => {
                      var that = this;
                      this.dialogVisible1 = true;
                      // alert(this.dialogVisible1)
                      this.userID = params.row.id;
                      console.log(this.userID);
                      this.axios({
                        url:
                          "http://122.112.253.28:8095/prod-api/basedata/bdormitory/" +
                          that.userID,
                        method: "GET",
                        headers: {
                          Authorization: window.sessionStorage.token
                        }
                      }).then(response => {
                        that.updateForm = response.data.data;
                      });
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
    var that = this;
    this.axios({
      method: "get",
      url: url + "/smartdor/sdhygiene/list?pageNum=1&pageSize=10",
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
  methods: {
    change(pageIndex) {
      var that = this;
      this.axios({
        method: "get",
        url:
          url +
          `/smartdor/sdhygiene/list?pageNum=${pageIndex}&pageSize=${that.pageSize}`,
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
    delClass(id) {
      this.$confirm(
        "是否确认删除宿舍编号为'" + id + "'的数据项?, 是否继续?",
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
            url: url + "/smartdor/sdhygiene/deleteByIds/" + id,
            headers: {
              Authorization: window.sessionStorage.token
            }
          }).then(delDate => {
            this.axios({
              method: "get",
              url: url + "/smartdor/sdhygiene/list?pageNum=1&pageSize=10",
              headers: {
                Authorization: window.sessionStorage.token
              }
            }).then(replace => {
              console.log(replace);
              this.data1 = replace.data.data.list;
            });
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    }
  }
};
</script>
<style>
.el-col-10 {
  width: 41.66667%;
  margin-left: 8px;
  margin-top: 11px;
}
.el-button--mini,
.el-button--mini.is-round {
  padding: 7px 15px;
  margin-left: -252px;
  margin-top: 12px;
}
/* .el-col-11 {
    width: 45.83333%;
    margin-left: 263px;
} */
.el-row {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  margin-top: 10px;
}
</style>
<style scoped>
</style>
