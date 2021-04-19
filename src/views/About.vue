<template>
  <div class="layout">
    <Layout>
      <Sider ref="side1" hide-trigger collapsible :collapsed-width="78" v-model="isCollapsed">
        <Menu
          active-name="5"
          theme="dark"
          width="auto"
          :class="menuitemClasses"
          v-if="list.length > 0"
        >
          <MenuItem name="4">
            <img src="../assets/2.png" alt class="tp1" />
            <span class="sp1">智慧宿管云平台</span>
          </MenuItem>
          <MenuItem name="3-1" to="/shouye">
            <Icon type="ios-navigate"></Icon>
            <span>首页</span>
          </MenuItem>
          <Submenu :name="key+2" v-for="(item, key) in list" :key="key">
            <template slot="title">
              <Icon type="ios-navigate"></Icon>
              <span>{{item.meta.title}}</span>
            </template>
            <MenuItem
              :name="(key+2)+'-'+(key2+1)"
              :to="''+item2.path+''"
              v-for="(item2, key2) in item.children"
              :key="key2"
            >{{item2.meta.title}}</MenuItem>
          </Submenu>
        </Menu>
      </Sider>
      <Layout>
        <Header :style="{padding: 0}" class="layout-header-bar">
          <Icon
            @click.native="collapsedSider"
            :class="rotateIcon"
            :style="{margin: '0 20px'}"
            type="md-menu"
            size="24"
            class="icon_1"
          ></Icon>
          <el-row :gutter="20" class="el_row_1" >
            <el-col :span="1" :offset="20">
              <div class="grid-content bg-purple"><i class="el-icon-search"></i></div>
            </el-col>
            <el-col :span="1">
              <div class="grid-content bg-purple"><i class="el-icon-rank"></i></div>
            </el-col>
            <el-col :span="1">
              <div class="grid-content bg-purple"><i class="el-icon-search"></i></div>
            </el-col>
            <el-col :span="1">
              <div class="grid-content bg-purple"><i class="el-icon-search"></i></div>
            </el-col>
          </el-row>
        </Header>

        <div class="router">
          <router-view></router-view>
        </div>
      </Layout>
    </Layout>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isCollapsed: false,
      breadList: [], // 路由集合
      list: []
    };
  },

  created() {},

  mounted() {
    this.loadData();
  },

  computed: {
    rotateIcon() {
      return ["menu-icon", this.isCollapsed ? "rotate-icon" : ""];
    },
    menuitemClasses() {
      return ["menu-item", this.isCollapsed ? "collapsed-menu" : ""];
    }
  },

  methods: {
    loadData() {
      var that = this;
      this.axios({
        url: "http://122.112.253.28:8095/prod-api/sys/menu/getRouters",
        method: "GET",
        headers: {
          Authorization: window.sessionStorage.token
        }
      })
        .then(response => {
          // console.log(response.data);
          that.list = response.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    collapsedSider() {
      this.$refs.side1.toggleCollapse();
    }
  }
};
</script>

<style scoped>
.layout {
  border: 1px solid #d7dde4;
  background: #f5f7f9;
  position: relative;
  border-radius: 4px;
  overflow: hidden;
}
.layout-header-bar {
  background: #fff;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
}
.layout-logo-left {
  width: 90%;
  height: 30px;
  background: #5b6270;
  border-radius: 3px;
  margin: 15px auto;
}
.menu-icon {
  transition: all 0.3s;
}
.rotate-icon {
  transform: rotate(-90deg);
}
.menu-item span {
  display: inline-block;
  overflow: hidden;
  width: 69px;
  text-overflow: ellipsis;
  white-space: nowrap;
  vertical-align: bottom;
  transition: width 0.2s ease 0.2s;
}
.menu-item i {
  transform: translateX(0px);
  transition: font-size 0.2s ease, transform 0.2s ease;
  vertical-align: middle;
  font-size: 16px;
}
.collapsed-menu span {
  width: 0px;
  transition: width 0.2s ease;
}
.collapsed-menu i {
  transform: translateX(5px);
  transition: font-size 0.2s ease 0.2s, transform 0.2s ease 0.2s;
  vertical-align: middle;
  font-size: 22px;
}
.router {
  width: 1685px;
  height: 870px;
  padding: 22px 22px 0px 22px;
}
.sp1 {
  position: relative;
  top: -10px;
  left: 10px;
}
.tp1 {
  width: 32px;
  height: 32px;
  margin-top: -5px;
}
.icon_1{
  height: 64px;
  line-height: 64px;
  float: left;
}
.el_row_1{
  width: 1647px;
  float: left;
}
</style>
