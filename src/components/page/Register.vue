<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">注册</div>
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
                <el-form-item prop="name">
                    <el-input v-model="param.name" placeholder="请输入用户名">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input
                        type="password"
                        placeholder="请输入密码"
                        v-model="param.password"
                        @keyup.enter.native="submitForm()"
                    >
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password1">
                    <el-input
                        type="password"
                        placeholder="请再次输入上面的密码"
                        v-model="password"
                    >
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm()">注册</el-button>
                </div>
                <el-row type="flex" justify="space-between" style="margin-top:8px">
                    <el-col :span="12" ></el-col>
                    <el-col :span="12" style="text-align:right"><router-link to="/login" class="operate">立即登录</router-link></el-col>
                </el-row>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            param: {
                name: '',
                password: '',
            },
            password:'',
            rules: {
                name: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
                password1: [{ required: true, message: '请再次输入密码', trigger: 'blur' }],
            },
        };
    },
    created(){
    },
    methods: {
        submitForm() {
            var that = this
            this.$refs.login.validate(valid => {
                if (valid) {
                    that.$message = this.$message
                    this.api.login({name:this.param.name,password:this.param.password}).then(res => {
                        console.log(res)
                        if(res.code=="200"){
                            localStorage.setItem('userInfo', JSON.stringify(res.data));
                            that.$router.push('/dashboard');
                            that.$message.success('登录成功');
                        }else{
                            that.$message.error(res.message);
                        }
                    })
                } else {
                    this.$message.error('请输入用户名和密码');
                    return false;
                }
            });
        },
    },
}
</script>

<style scoped>
.login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
    background-image: url(../../assets/img/bg.png);
    background-size: 100%;
}
.ms-title {
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    color: #fff;
    border-bottom: 1px solid #ddd;
}
.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 350px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    background: rgba(255, 255, 255, 0.3);
    overflow: hidden;
}
.ms-content {
    padding: 30px 30px;
}
.login-btn {
    text-align: center;
}
.login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
}
.login-tips {
    font-size: 12px;
    line-height: 30px;
    color: #fff;
}
.operate {
    text-align:left;
    font-size:13px;
    color:#303133;
}
</style>