<template>
  <div class="register">
    <el-form class="register-form" ref="registerForm" :model="registerForm" label-width="100px" size="mini">
      <div class="title"><span>实用功能营养平台从业者账号申请</span></div>
      <div class="sub-title"><span>机构信息</span></div>
      <el-form-item label="机构类型：">
        <el-radio v-model="registerForm.type" label="1">机构用户</el-radio>
        <el-radio v-model="registerForm.type" label="2">非机构用户</el-radio>
      </el-form-item>
      <el-form-item label="公司名称：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="公司地址：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="公司电话：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="公司税号：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="银行账号：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="开户行：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="证照信息：">
        <el-upload
          action="#"
          list-type="picture-card"
          :auto-upload="false">
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{file}">
            <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" >
            <span class="el-upload-list__item-actions">
              <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)" >
                <i class="el-icon-zoom-in"></i>
              </span>
              <span v-if="!licenseDisabled" class="el-upload-list__item-delete" @click="handleDownload(file)" >
                <i class="el-icon-edit"></i>
              </span>
              <span v-if="!licenseDisabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
        </el-upload>
      </el-form-item>
      <div class="sub-title"><span>商户信息</span></div>
      <el-form-item label="外显名称：">
        <el-input v-model="registerForm.name"></el-input>
      </el-form-item>
      <el-form-item label="公司logo：">
        <el-upload
          action="#"
          list-type="picture-card"
          :auto-upload="false">
          <i slot="default" class="el-icon-plus"></i>
          <div slot="file" slot-scope="{file}">
            <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" >
            <span class="el-upload-list__item-actions">
              <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)" >
                <i class="el-icon-zoom-in"></i>
              </span>
              <span v-if="!logoDisabled" class="el-upload-list__item-delete" @click="handleDownload(file)" >
                <i class="el-icon-edit"></i>
              </span>
              <span v-if="!logoDisabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
                <i class="el-icon-delete"></i>
              </span>
            </span>
          </div>
        </el-upload>
      </el-form-item>
      <el-form-item label="采购地址：">
        <el-button icon	="el-icon-plus" @click="addDomain"></el-button>
      </el-form-item>

      <div
        v-for="(domain, index) in purchases"
        :key="index">
        <el-form-item>
          <span slot="label">地址 {{index + 1}}：</span>
          <el-col :span="12">
            <el-row>
              <el-form-item label="联系人：">
                <el-input v-model="purchases[index].contact"></el-input>
              </el-form-item>
            </el-row>
          </el-col>
          <el-col :span="12">
            <el-form-item label="联系电话：">
              <el-input v-model="purchases[index].contactPhone"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="身份证号：">
              <el-input v-model="purchases[index].idCard"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item label="邮寄地址：">
              <el-input v-model="purchases[index].address"></el-input>
            </el-form-item>
          </el-col>
        </el-form-item>
      </div>


      <div class="sub-title"><span>管理员账号</span></div>
      <el-row>
        <el-col :span="12">
          <el-form-item label="姓名：">
            <el-input v-model="registerForm.value"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="手机号：">
            <el-input v-model="registerForm.value"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <el-form-item>
        <el-button type="primary" @click="handleLogin">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
    <el-dialog :visible.sync="dialogVisible">
      <img width="100%" :src="dialogImageUrl" alt="">
    </el-dialog>
    <!--  底部  -->
<!--    <div class="el-login-footer">-->
<!--      <span>Copyright © 2019-2020 <a href="http://jeethink.vip">jeethink.vip</a> All Rights Reserved.</span>-->
  </div>
</template>

<script>

export default {
  name: "Register",
  data() {
    return {
      codeUrl: "",
      cookiePassword: "",
      purchasesLabel: '地址1：',
      purchases: [{
        contact: '',
        contactPhone: '',
        idCard: '',
        address: '',
      }],
      registerForm: {
        /*username: "admin",
        password: "admin123",*/
        type: "1",
        username: "",
        password: "",
        rememberMe: false,
        code: "",
        uuid: ""
      },
      dialogImageUrl: '',
      dialogVisible: false,
      licenseDisabled: false,
      logoDisabled: false,
      logoPlusDisabled: true,
      registerRules: {
        username: [
          { required: true, trigger: "blur", message: "用户名不能为空" }
        ],
        password: [
          { required: true, trigger: "blur", message: "密码不能为空" }
        ],
        code: [{ required: true, trigger: "change", message: "验证码不能为空" }]
      },
      loading: false,
      redirect: undefined
    };
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },

  created() {

  },
  methods: {
    addDomain() {
      this.purchasesLabel = '地址' + this.purchases.length;
      this.purchases.push({
        contact: '',
        contactPhone: '',
        idCard: '',
        address: '',
      });
    },
    accountRegister() {
      // this.$router.push({ path: "/index" });
      location.href = '/index';
    },
    handleLogin() {
      this.$refs.registerForm.validate(valid => {
        if (valid) {
          this.loading = true;
          if (this.registerForm.rememberMe) {
            Cookies.set("username", this.registerForm.username, { expires: 30 });
            Cookies.set("password", encrypt(this.loginForm.password), { expires: 30 });
            Cookies.set('rememberMe', this.loginForm.rememberMe, { expires: 30 });
          } else {
            Cookies.remove("username");
            Cookies.remove("password");
            Cookies.remove('rememberMe');
          }
          this.$store
            .dispatch("Login", this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || "/" });
            })
            .catch(() => {
              this.loading = false;
              this.getCode();
            });
        }
      });
    },
    handleRemove(file) {
      console.log(file);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleDownload(file) {
      console.log(file);
    }
  }
};
</script>

<style rel="stylesheet/scss" lang="scss">
.register {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100%;
  background-image: url("../assets/image/login-background.jpg");
  background-size: cover;
}
.register-form {
  background-color: #FFF;
  height: 100%;
  width: 50%;
  padding: 0 150px 30px 150px;
  //margin-bottom: 20px;
}
.title {
  height: 60px;
  margin: 20px 0;
  line-height: 60px;
  font-size: 20px;
  text-align: center;
  font-weight: bold;
}
.sub-title {
  height: 20px;
  margin: 15px 0;
  line-height: 20px;
  font-size: 16px;
  padding-left: 18px;
  font-weight: bold;
}
.el-form-item {
  margin-bottom: 10px;
}
</style>
