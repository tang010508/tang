<template>
  <div>
    <div class="nav-top">
      <label class="lab">宿舍栋号</label>
      <Select style="width:200px" class="sele" v-model="buildingNo">
        <Option v-for="(item,key) in list" :key="key" :value="item.dictSort">{{item.dictLabel}}</Option>
      </Select>
      <label class="lab">楼层</label>
      <Select style="width:200px" class="sele" v-model="storey">
        <Option v-for="(item,key) in list2" :key="key" :value="item.dictSort">{{item.dictLabel}}</Option>
      </Select>
      <label class="lab">宿舍号</label>
      <Input placeholder="请输入宿舍号" style="width: 180px" v-model="dormitoryNo"/>
      <label class="lab">状态</label>
      <Select style="width:200px" class="sele">
        <Option v-for="(item,key) in list3" :key="key" :value="item.dictLabel">{{item.dictLabel}}</Option>
      </Select>
      <el-button type="primary" icon="el-icon-search" style="margin-left:20px;" @click="showNr">搜索</el-button>
      <el-button plain icon="el-icon-refresh">重置</el-button>
    </div>
    <el-row class="el_row_1">
      <el-button type="primary" icon="el-icon-plus" @click="dialogFormVisible = true">新增</el-button>
      <el-button type="success" icon="el-icon-edit">修改</el-button>
      <el-button type="danger" icon="el-icon-delete">删除</el-button>
    </el-row>
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
              <el-switch v-model="scope.row.status" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
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

    <!-- 增加 -->
    <el-dialog title="修改宿舍信息" :visible.sync="dialogFormVisible">
      <el-form ref="from2" :model="from" label-width="80px">
        <el-form-item label="宿舍栋号">
          <el-select v-model="from.buildingNo" placeholder="请选择" style="width:350px;">
            <el-option
              v-for="(item, key) in list"
              :key="key"
              :value="item.dictSort"
            >{{ item.dictLabel }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="楼层">
          <el-select v-model="from.storey" placeholder="请选择" style="width:350px;">
            <el-option
              v-for="(item, key) in list2"
              :key="key"
              :value="item.dictSort"
            >{{ item.dictLabel }}</el-option>
          </el-select>
        </el-form-item>
        <label class="lab">宿舍号</label>
        <Input
          placeholder="请输入宿舍号"
          style="width: 350px;margin-left:6px;margin-bottom: 15px;height: 40px;"
          v-model="from.dormitoryNo"
        />
        <el-form-item label="宿管老师">
          <el-select v-model="from.dormitoryTeacherId" placeholder="请选择" style="width:350px;">
            <el-option v-for="(item, key) in list4" :key="key" :value="item.id">{{ item.username }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">确定</el-button>
          <el-button @click="xsqx">取消</el-button>
        </el-form-item>
      </el-form>
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
              v-for="(item, key) in list4"
              :key="key"
              :value="item.id"
              :label="item.dormitoryTeacherName"
            >{{ item.username }}</el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit2">确定</el-button>
          <el-button @click="xsqx">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      pageNum: 1,
      pageSize: 10,
      dialogFormVisible2: false,
      dialogFormVisible: false,
      list: [],
      list2: [],
      list3: [],
      list4: [],
      jb: [],
      multipleSelection: [],
      from2: {
        dormitoryTeacherId: "",
        username: []
      },
      from: {
        buildingNo: [],
        storey: [],
        dormitoryTeacherId: [],
        dormitoryNo: ""
      }
    };
  },
  mounted() {
    this.loadData();
    this.loadData2();
    this.loadData3();
    this.loadData4();
    this.show();
  },
  methods: {
    // 宿舍栋号
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
    // 楼层
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
    // 状态
    loadData3() {
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
          console.log(response.data);
          that.list3 = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    // 显示
    show() {
      var that = this;
      this.axios({
        // id:that.jb.id,
        url: `http://122.112.253.28:8095/prod-api/basedata/bdormitory/list?pageNum=1&pageSize=10`,
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
    // 宿管老师
    loadData4() {
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
          that.list4 = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    // 删除
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
        // console.log(response)
        if (response.data.code == "200") {
          this.$message({
            type: "success",
            message: "删除成功!"
          });
          this.show();
        } else {
          this.$message({
            type: "info",
            message: "删除失败"
          });
        }
      });
    },
    // 获取id内数据内容
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
          // that.success(response.data.message);
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
            this.show();
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
    xsqx() {
      this.dialogFormVisible = false;
      this.dialogFormVisible2 = false;
    },
    // 增加
    onSubmit() {
      var that = this;
      this.axios({
        url: "http://122.112.253.28:8095/prod-api/basedata/bdormitory/create",
        method: "POST",
        data: {
          buildingNo: that.from.buildingNo,
          storey: that.from.storey,
          dormitoryNo: that.from.dormitoryNo,
          dormitoryTeacherId: that.from.dormitoryTeacherId
        },
        headers: {
          Authorization: window.sessionStorage.token
        }
      }).then(response => {
        if (response.data.code == "200") {
          this.$message({
            type: "success",
            message: "添加成功"
          });
          this.dialogFormVisible = false;
          this.show();
        } else {
          this.$message({
            type: "info",
            message: "添加失败"
          });
        }
      });

      this.from = {
        buildingNo: "",
        storey: "",
        dormitoryTeacherId: "",
        dormitoryNo: ""
      };
    },
    // 查找
    showNr(){
      var that = this;
      this.axios({
        url: `http://122.112.253.28:8095/prod-api/basedata/bdormitory/list?pageNum=1&pageSize=10&buildingNo=${that.buildingNo}&storey=${that.storey}&dormitoryNo=${that.dormitoryNo}`,
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        },
        data: {
          buildingNo:that.buildingNo,
          storey: that.storey,
          dormitoryNo: that.dormitoryNo
        }
      })
        .then(response => {
          that.jb = response.data.data.list;
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>
<style scoped>
.nav-top {
  margin: 32px;
  height: 45px;
}
.lab {
  width: 68px;
  font-size: 14px;
  color: #606266;
  padding: 0 12px 0 0;
  margin-left: 20px;
}
.el_row_1 {
  margin: 0 32px;
}
</style>
