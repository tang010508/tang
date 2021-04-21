<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>基础数据</el-breadcrumb-item>
      <el-breadcrumb-item>班级管理</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图区域 -->
    <el-card>
      <!-- 搜索与添加区域 -->
      <el-row :gutter="30">
        <el-col :span="5">
          <el-input
            placeholder="请输入学员名称"
            v-model="queryInfo.studentName"
            clearable
            @clear="getUserList"
          >
            <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
          </el-input>
        </el-col>
        <el-col :span="5">
          <el-input
            placeholder="请输入学号"
            v-model="queryInfo.studentNo"
            clearable
            @clear="getUserList"
          >
            <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
          </el-input>
        </el-col>
        <el-col :span="5">
          <div>
            <!-- <el-select slot="prepend" placeholder="请选择">
              <el-option label="正常" value="1"></el-option>
              <el-option label="停用" value="2"></el-option>
            </el-select>-->
          </div>
        </el-col>
        <el-col :span="8">
          <el-button type="primary" @click="addDialogVisible = true">新增</el-button>
          <el-button type="success" icon="el-icon-edit" disabled>修改</el-button>
          <el-button type="danger" icon="el-icon-delete" disabled>删除</el-button>
        </el-col>
      </el-row>

      <!-- 班级列表区域 -->
      <el-table :data="userlist" border stripe>
        <el-table-column label="#" type="index"></el-table-column>
        <el-table-column label="序号" prop="id"></el-table-column>
        <el-table-column label="学员姓名" prop="studentName"></el-table-column>
        <el-table-column label="学号" prop="studentNo"></el-table-column>
        <el-table-column label="班级" prop="className"></el-table-column>
        <el-table-column label="手机号" prop="phone"></el-table-column>
        <el-table-column label="性别" prop="sex"></el-table-column>
        <el-table-column label="家长姓名" prop="parentName"></el-table-column>
        <el-table-column label="家长电话" prop="parentPhone"></el-table-column>
        <el-table-column label="宿舍栋号" prop="buildingNo"></el-table-column>
        <el-table-column label="楼层" prop="storey"></el-table-column>
        <el-table-column label="宿舍号" prop="dormitoryNo"></el-table-column>
        <el-table-column label="创建时间" prop="createTime"></el-table-column>
        <el-table-column label="状态">
          <template slot-scope="scope">
            <el-switch v-model="scope.row.status" @change="userStateChanged(scope.row)"></el-switch>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <!-- 修改 -->
            <el-tooltip effect="dark" content="修改" placement="top" :enterable="false">
              <el-button
                type="primary"
                icon="el-icon-edit"
                circle
                @click="showEditDialog(scope.row.id)"
              ></el-button>
            </el-tooltip>
            <!-- 删除 -->
            <el-button
              type="danger"
              icon="el-icon-delete"
              circle
              @click="removeUserById(scope.row.id)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 分页区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pageNum"
        :page-sizes="[1, 2, 5, 10]"
        :page-size="queryInfo.pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>

    <!--添加班级列表的对话框 -->
    <el-dialog title="添加班级信息" :visible.sync="addDialogVisible" width="40%" @close="addDialogClosed">
      <!-- 内容主体区域 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="100px">
        <el-form-item label="班级名称" prop="className">
          <el-input v-model="addForm.className"></el-input>
        </el-form-item>
        <el-form-item label="班主任" prop="classTeacherName">
          <el-input v-model="addForm.classTeacherName" disabled></el-input>
        </el-form-item>
      </el-form>
      <!--底部区域 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addUser">确 定</el-button>
      </span>
    </el-dialog>

    <!-- 修改班级列表的对话框 -->
    <el-dialog
      title="修改班级信息"
      :visible.sync="editDialogVisible"
      width="50%"
      @close="editDialogClosed"
    >
      <el-form ref="editFormRef" :model="editForm" :rules="editFormRules" label-width="70px">
        <el-form-item label="班级名称">
          <el-input v-model="editForm.className" disabled></el-input>
        </el-form-item>
        <el-form-item label="班主任">
          <el-input v-model="editForm.classTeacherName"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="deitUserInfo">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //获取用户列表的参数对象
      queryInfo: {
        //当前的页数
        pageNum: 1,
        //当前显示多少条数据
        pageSize: 10,
        total: 38,
        totalPage: 4
      },
      //控制添加班级对话框的显示与隐藏
      addDialogVisible: false,
      //添加班级列表的表单数据
      addForm: {
        // className: '',
        classTeacherName: "admin",
        className: "",
        classTeacherId: 1,
        status: '1'
      },
      //添加表单的验证规则对象
      addFormRules: {
        className: [
          { required: true, message: '请输入班级名称', trigger: 'blur' },
          {
            min: 1,
            max: 10,
            message: '班级名称到1~10个字符之间',
            trigger: 'blur'
          }
        ],
        classTeacherName: [
          { required: true, message: '请输入班主任名称', trigger: 'blur' },
          {
            min: 1,
            max: 10,
            message: '班主任名称到1~10个字符之间',
            trigger: 'blur'
          }
        ]
      },
      userlist: [],
      total: 0,
      //控制修改班级列表的显示与隐藏
      editDialogVisible: false,
      //查询到的用户信息对象
      editForm: {
        // className: '',
        // classTeacherId: 0,
        classTeacherName: "",
        // createTime: '',
        // id: 0,
        // modifyTime: '',
        // status: ''
      },
      editFormRules: {}
    }
  },
  created() {
    this.getUserList()
  },
  methods: {
    async getUserList() {
      const { data: res } = await this.$http.get('/basedata/bstudent/list', {
        params: this.queryInfo
      })
      if (res.code !== 200) {
        return this.$message.error('获取班级列表失败!')
      }
      this.userlist = res.data.list
      this.total = res.data.total
      console.log(res)
    },
    //监听 pagesize 改变的事件
    handleSizeChange(newSize) {
      // console.log(newSize);
      this.queryInfo.pageSize = newSize
      this.getUserList()
    },
    // 监听 页码值 改变的事件
    handleCurrentChange(newPage) {
      console.log(newPage)
      this.queryInfo.pageNum = newPage
      this.getUserList()
    },
    //监听 switch开关状态的改变
    async userStateChanged(userinfo) {
      console.log(userinfo)
      const { data: res } = await this.$http.put(
        `/basedata/bclass/update/status/` ,
         this.addForm

      )
      if (res.code !== 200) {
        userinfo.code = !userinfo.code
        return this.$message.error('更新用户状态失败!')
      }
      this.$message.success('更新用户成功')
    },
    //监听添加班级对话框的关闭事件
    addDialogClosed() {
      this.$refs.addFormRef.resetFields()
    },
    //点击按钮，添加新用户
    addUser() {
      this.$refs.addFormRef.validate(async valid => {
        if (!valid) return
        //可以发起添加用户的网络请求
        const { data: res } = await this.$http.post(
          '/basedata/bclass/create',
          this.addForm
        )
        if (res.code !== 200) {
          this.$message.errop('添加用户失败')
        }
        this.$message.success('添加用户成功!')
        //隐藏添加班级列表的对话框
        this.addDialogVisible = false
        //重新获取班级列表
        this.getUserList()
      })
    },
    //展示编辑班级列表的对话框
    async showEditDialog(id) {
      // console.log(id);
      const { data: res } = await this.$http.get('/basedata/bclass/' + id)
      if (res.code !== 200) {
        return this.$message.error('查询用户信息失败!')
      }
      this.editForm = res.data
      this.editDialogVisible = true
    },
    //监听修改用户对话框的关闭事件
    editDialogClosed() {
      this.$refs.editFormRef.resetFields()
    },
    //修改用户信息并提交
    deitUserInfo() {
      this.$refs.editFormRef.validate(async valid => {
        if (!valid) return
        //发起修改用户信息的数据请求
        const { data: res } = await this.$http.put(
          '/basedata/bclass/update/' + this.editForm.id,
          {
            // className:this.editForm.className,
            classTeacherName: this.editForm.classTeacherName
          }
        )
        if (res.code !== 200) {
          return this.$message.error('更新用户信息失败！')
        }
        //关闭对话框
        this.editDialogVisible = false
        //刷新数据列表
        this.getUserList()
        //提升修改成功
        this.$message.success('更新用户信息成功')
      })
    },
    //根据Id删除
    async removeUserById(id) {
      //弹框询问用户是否删除数据
      const confirmResult = await this.$confirm(
        '此操作将永久删除该列表, 是否继续?',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }
      ).catch(err => err)
      // console.log(confirmResult);
      if (confirmResult !== 'confirm') {
        return this.$message.info('已取消删除')
      }
      const { data: res } = await this.$http.delete(
        '/basedata/bclass/deleteByIds/' + id
      )
      if (res.code !== 200) {
        return this.$message.error('删除用户失败！')
      }
      this.$message.success('删除用户成功!')
      this.getUserList()
    }
  }
}
</script>
<style lang="less" scoped>
</style>
