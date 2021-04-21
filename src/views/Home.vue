<template>
  <div class="dengl">
    <div class="div_d">
      <h3 class="h3_h">智慧宿管云平台</h3>
        <el-input placeholder="请输入账户" v-model="username" name="username" class="el_input_1">
          <i slot="prefix" class="el-icon-user-solid biao1"></i>
        </el-input>

        <el-input
          placeholder="请输入密码"
          v-model="password"
          type="password"
          name="password"
          class="el_input_1"
        >
          <i slot="prefix" class="el-icon-s-goods biao1"></i>
        </el-input>

        <el-input placeholder="验证码" v-model="code" name="code" class="el_input_2">
          <i slot="prefix" class="iconfont biao1">&#xe624;</i>
        </el-input>

        <img :src="imgurl" class="img_i" @click="qiehuan" />
      <br />
      <!-- `checked` 为 true 或 false -->
      <el-checkbox v-model="checked1" class="el_checkbox_1">记住密码</el-checkbox>
      <el-row class="el_row1">
        <el-button type="primary" @click="dengl" class="el_button_1" v-on:keyup.enter="username">登录</el-button>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "admin",
      password: "",
      imgurl: "",
      uuid: "",
      code: "",
      checked1: true,
    };
  },
  name: "SIdentify",
  props: {
    identifyCodes: {
      type: String,
      default: "1234"
    },
    fontSizeMin: {
      type: Number,
      default: 25
    },
    fontSizeMax: {
      type: Number,
      default: 30
    },
    backgroundColorMin: {
      type: Number,
      default: 255
    },
    backgroundColorMax: {
      type: Number,
      default: 255
    },
    colorMin: {
      type: Number,
      default: 0
    },
    colorMax: {
      type: Number,
      default: 160
    },
    lineColorMin: {
      type: Number,
      default: 100
    },
    lineColorMax: {
      type: Number,
      default: 255
    },
    dotColorMin: {
      type: Number,
      default: 0
    },
    dotColorMax: {
      type: Number,
      default: 255
    },
    contentWidth: {
      type: Number,
      default: 112
    },
    contentHeight: {
      type: Number,
      default: 31
    }
  },
  created() {
    this.axios({
      method: "get",
      url: "http://122.112.253.28:8095/prod-api/captchaImage"
    }).then(result => {
      this.$data.imgurl = "data:image/gif;base64," + result.data.data.img;
      this.$data.uuid = result.data.data.uuid;
    });
  },
  methods: {
    qiehuan() {
      this.created();
    },
    created() {
      this.axios({
        method: "get",
        url: "http://122.112.253.28:8095/prod-api/captchaImage"
      }).then(result => {
        this.$data.imgurl = "data:image/gif;base64," + result.data.data.img;
        this.$data.uuid = result.data.data.uuid;
      });
    },
    dengl(values) {
      this.axios({
        method: "post",
        url: "http://122.112.253.28:8095/prod-api/admin/login",
        data: {
          username: this.$data.username,
          password: this.$data.password,
          code: this.$data.code,
          uuid: this.$data.uuid
        }
      }).then(result => {
        console.log(result);
        if (result.data.code == 200) {
          // sessionStorage.setItem("token", result.data.data.token);
          window.sessionStorage.token =result.data.data.tokenHead + " " + result.data.data.token;
          this.$router.push({
            path: "About"
          });
          this.$message({
            message: result.data.message,
            type: "success"
          });
        } else {
          this.$message({
            message: result.data.message,
            type: "success"
          });
        }
      });
    },
  }
};
</script>

<style scoped>
@font-face {
  font-family: "iconfont"; /* project id 2459112 */
  src: url("//at.alicdn.com/t/font_2459112_uyugqy2z16.eot");
  src: url("//at.alicdn.com/t/font_2459112_uyugqy2z16.eot?#iefix")
      format("embedded-opentype"),
    url("//at.alicdn.com/t/font_2459112_uyugqy2z16.woff2") format("woff2"),
    url("//at.alicdn.com/t/font_2459112_uyugqy2z16.woff") format("woff"),
    url("//at.alicdn.com/t/font_2459112_uyugqy2z16.ttf") format("truetype"),
    url("//at.alicdn.com/t/font_2459112_uyugqy2z16.svg#iconfont") format("svg");
}
.dengl {
  width: 100%;
  height: 936px;
  background: url("../assets/1.png") no-repeat;
  background-size: 100% 100%;
}
.div_d {
  position: relative;
  left: 750px;
  top: 228px;
  border-radius: 6px;
  background: #fff;
  width: 400px;
  height: 400px;
  padding: 25px 25px 5px 25px;
}
.h3_h {
  text-align: center;
  color: #707070;
}
.el_input_1 {
  margin-top: 30px;
}
.biao1 {
  font-size: 25px;
  line-height: 40px;
}
.el_row1 {
  width: 350px;
  height: 36px;
  background-color: #1890ff;
  margin: 0 auto;
  border-radius: 4px;
  margin-top: 22px;
}
.el_row1:hover {
  opacity: 0.8;
  cursor: pointer;
}
.el_button_1 {
  width: 350px;
  height: 36px;
  text-align: center;
  color: #fff;
  font-size: 14px;
}
.el_input_2 {
  width: 220px;
  margin-top: 30px;
}
.img_i {
  width: 110px;
  height: 40px;
  margin-left: 10px;
  top: 15px;
  position: relative;
}
.div_2 {
  height: 75px;
}
.el_checkbox_1{
  margin-top: 25px;
}
</style>
