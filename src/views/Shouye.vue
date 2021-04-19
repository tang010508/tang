<template>
  <div>
    <div class="nav">
      <button class="btn1">开闸</button>
      <div class="xk">
        <label class="lab">宿舍栋号</label>
        <Select style="width:200px" class="sele" v-model="checkDate">
          <Option v-for="(item,key) in list" :key="key" :value="item.dictLabel">{{item.dictLabel}}</Option>
        </Select>
        <label class="lab">楼层</label>
        <Select style="width:200px" class="sele" v-model="building">
          <Option v-for="(item,key) in list2" :key="key" :value="item.dictLabel">{{item.dictLabel}}</Option>
        </Select>
        <label class="lab">宿舍号</label>
        <Input placeholder="请输入宿舍号" style="width: 180px" v-model="storey" />
        <br />
        <br />
        <label class="lab">请选择日期</label>
        <DatePicker type="date" :options="options1" placeholder="请选择日期时间" style="width: 200px"></DatePicker>
        <button class="sub" @click="cx">查询</button>
      </div>
      <div class="nav-qh">
        <div class="onepx">
          <div class="zt" @click="zk">
            <span class="sp1" v-show="!showZk">缺勤学生详情，请点击展开</span>
            <span class="sp1" v-show="showZk">缺勤学生详情，请点击折叠</span>
          </div>
        </div>
      </div>
    </div>

    <div class="ss-xx" v-show="!showZk">
      <div class="sslc" v-for="(item, key) in list3" :key="key">
        <div class="nav-ss">
          <span class="sp2">{{item.roomName}}</span>
          <span class="sp3">总宿舍人数{{item.total}}</span>
        </div>
        <div class="zt-ss">
          <el-badge :value="item.normal" class="item">
            <el-button size="small">正常</el-button>
          </el-badge>
          <el-badge :value="item.leaveCount" class="item" type="primary">
            <el-button size="small">请假</el-button>
          </el-badge>
          <el-badge :value="item.comebacklate" class="item" type="warning">
            <el-button size="small">晚归</el-button>
          </el-badge>
          <el-badge :value="item.absence" class="item">
            <el-button size="small">缺勤</el-button>
          </el-badge>
        </div>
      </div>
    </div>
    <div class="ss-xx" v-show="showZk"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      list: [],
      list2: [],
      list3: [],
      checkDate: "",
      building: "",
      storey: "",
      showZk: false,
      options1: {
        shortcuts: [
          {
            text: "Today",
            value() {
              return new Date();
            },
            onClick: picker => {
              this.$Message.info("Click today");
            }
          },
          {
            text: "Yesterday",
            value() {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              return date;
            },
            onClick: picker => {
              this.$Message.info("Click yesterday");
            }
          },
          {
            text: "One week",
            value() {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
              return date;
            },
            onClick: picker => {
              this.$Message.info("Click a week ago");
            }
          }
        ]
      },
      jb:[]
    };
  },
  mounted() {
    this.loadData();
    this.loadData2();
    this.loadData3();
  },
  methods: {
    loadData() {
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
          console.log(response.data);
          that.list = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    loadData2() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/sys/dict/detail/dictType/sys_storey",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          console.log(response.data);
          that.list2 = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    loadData3() {
      var that = this;
      this.axios({
        url:
          "http://122.112.253.28:8095/prod-api/smartdor/sdAttence/listAbsenceDormitory?checkDate=&building=&storey=&bdormitory=",

        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          console.log(response.data);
          that.list3 = response.data.data.list;
        })
        .catch(error => {
          console.log(error);
        });
    },
    zk() {
      if ((this.showZk = !this.showZk)) {
        this.showZk = true;
      }
    },
    cx() {
      var that = this;
      that.axios({
        url: `http://122.112.253.28:8095/prod-api/smartdor/sdAttence/listAbsenceDormitory?checkDate=&building=&storey=&bdormitory=${that.storey}`,
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        },
        data: {
          roomName:that.storey
        }
      }).then(response =>{
        console.log(response)
        that.list3 = response.data.data.list;
      })
    }
  }
};
</script>
<style scoped>
.item {
  margin-left: 23px;
  margin-bottom: 20px;
}
.zt-ss {
  padding: 15px 20px 20px 20px;
  font-size: 12px;
}
.sp2 {
  font-weight: 700;
  font-size: 16px;
  color: #606266;
}
.sp3 {
  font-weight: 700;
  font-size: 16px;
  color: #606266;
  float: right;
  padding: 3px 0px;
}
.nav-ss {
  padding: 14px 15px 7px;
  border-bottom: 1px solid #e6ebf5;
}
.sslc {
  float: left;
  margin: 5px;
  width: 226px;
  color: #606266;
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
}
.nav {
  margin: 32px;
  height: 200px;
}
.btn1 {
  background-color: red;
  color: white;
  width: 70px;
  cursor: pointer;
  height: 36px;
  line-height: 36px;
  text-align: center;
  outline: none;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
}
.lab {
  width: 85px;
  padding: 0 12px 0 0;
  font-size: 14px;
  text-align: right;
  display: inline-block;
  color: #606266;
}
.xk {
  text-align: center;
  margin-bottom: 20px;
}
.sele {
  text-align: left;
}
.sub {
  color: #fff;
  background-color: #1890ff;
  border: 1px solid #1890ff;
  outline: none;
  font-size: 14px;
  border-radius: 4px;
  width: 70px;
  height: 36px;
  cursor: pointer;
  margin-left: 20px;
  text-align: center;
  line-height: 36px;
}
.nav-qh {
  display: flex;
  align-items: center;
  height: 48px;
  line-height: 48px;
  background-color: #fff;
  color: #303133;
  border-bottom: 1px solid #e6ebf5;
  font-size: 13px;
  font-weight: 500;
  outline: none;
}
.onepx {
  display: block;
  height: 1px;
  width: 100%;
  margin: 24px 0;
  background-color: #dcdfe6;
  position: relative;
}
.zt {
  position: absolute;
  background-color: #fff;
  padding: 0 20px;
  font-weight: 500;
  color: #303133;
  font-size: 14px;
  cursor: pointer;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}
.sp1 {
  height: 28px;
  line-height: 26px;
  font-size: 16px;
  background-color: #e8f4ff;
  border: 1px solid #d1e9ff;
  display: inline-block;
  padding: 0 10px;
  color: #1890ff;
}
.ss-xx {
  width: 100%;
  overflow: hidden;
  margin: 32px;
}
</style>
