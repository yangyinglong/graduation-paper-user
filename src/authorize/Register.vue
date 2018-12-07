<template>
	<div class="register-main">
		<el-form label-position='left' :model="registerData" status-icon :rules="rules" ref="registerData" label-width="100px" class="demo-ruleForm">
			<el-form-item label="学号：" prop="studentId">
            	<el-input v-model="registerData.studentId" placeholder="用户登录"></el-input>
            </el-form-item>
            <el-form-item label="姓名：" prop="name">
            	<el-input v-model="registerData.name" placeholder="请输入姓名"></el-input>
            </el-form-item>
            <el-form-item label="手机号码：" prop="phone">
            	<el-input v-model="registerData.phone" placeholder="用户登录和找回密码"></el-input>
            </el-form-item>
			<el-form-item label="密码：" prop="password">
				<el-input type="password" v-model="registerData.password" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="确认密码：" prop="checkPass">
				<el-input type="password" v-model="registerData.checkPass" autocomplete="off"></el-input>
			</el-form-item>
			<el-form-item label="验证码：" prop="validatecode">
				<el-input v-model.number="registerData.validatecode" autocomplete="off" placeholder="请输入验证码">
					<div v-if="!sendMsgDisabled" slot="append" @click="sendSms(registerData.phone)" style="width: 100px">{{sendVali}}</div>
					<div v-if="sendMsgDisabled" slot="append" style="width: 100px">{{time+'秒后获取'}}</div>
				</el-input>
				<br>
				<el-checkbox v-model="checked">阅读并接受<u>《杭电实验室协议》</u></el-checkbox>
				<br><br>
				<el-button style="width:310px; float: right; background-color: #d7d7d7;" @click="register('registerData')">
				注册
				</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
	export default{
		name: 'Register',
		data(){
			var validatePass = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请输入密码'));
				} else {
					if (this.registerData.checkPass !== '') {
						this.$refs.registerData.validateField('checkPass');
					}
				callback();
				}
			};
			var validatePass2 = (rule, value, callback) => {
				if (value === '') {
					callback(new Error('请再次输入密码'));
				} else if (value !== this.registerData.pass) {
					callback(new Error('两次输入密码不一致!'));
				} else {
					callback();
				}
			};
			return{
				registerData: {
					studentId: '',
					name: '',
					phone: '',
					password: '',
					checkPass: '',
					validateCode: ''
				},
				checked: false,
				rules: {
					studentId:[
						{required: true, message: '学号不能为空', trigger: 'blur'},
			            {min: 8, max: 8, message: '学号长度为8位数字值', trigger: 'blur'}
					],
					name:[
						{required: true, message: '姓名不能为空', trigger: 'blur'}
					],
					phone: [
			            {required: true, message: '手机号码不能为空', trigger: 'blur'},
			            {min: 11, max: 11, message: '手机号码长度为11位数字值', trigger: 'blur'}
		        	],
					password: [
						{required: true, validator: validatePass, trigger: 'blur'}
					],
					checkPass: [
						{required: true, validator: validatePass2, trigger: 'blur'}
					],
					validateCode: [
						{required: true, message: '短信验证码不能为空', trigger: 'blur'}
					]
				},
				sendMsgDisabled: false,
				time: 60,
				sendVali: '获取短信验证码'
			}
		},
		created() {
			// this.GLOBAL.authorizeView = true
			this.GLOBAL.setAuthorizeView(true)
			console.log(this.GLOBAL.authorizeView)
		},
		methods:{
			sendSms(phone){
				if (phone.length === 11) {
					// 调用后端发送短信接口
					this.$message.success('已发送验证码！请查看手机。')
					let me = this
					me.sendMsgDisabled = true
					let interval = window.setInterval(function() {
						if ((me.time--) <= 0) {
							me.time = 60
							me.sendMsgDisabled = false         
							me.mess = '重新获取验证码'
							window.clearInterval(interval);
						}
					}, 1000);
				} else {
					this.$message.error('请填写正确的手机号码')
				}
			},
			register(registerData){
				
			}
		}
	}
	
</script>

<style scoped>
	.register-main{
		width: 400px;
		height: 530px;
		margin: 0 auto;
	}
</style>