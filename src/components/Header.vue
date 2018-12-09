<template>
	<div class="app-head">
		<div class="app-head-inner">
			<router-link to="/">
				<img src="../assets/logo_1.png" alt="">
			</router-link>
			<!-- <span>杭州电子科技大学实验室预约</span> -->
			<img src="../assets/motto.png" class="motto" style="width: 350px">		
			<div class="head-nav">
				<ul>
					<li style="margin-top: -1px">
						<Login v-on:changeStatus="login"/>
					</li>
					<li class="nav-pile">|</li>
					<li v-if="loginStatus===0">
						<router-link to="/register" style="text-decoration: none;">注册</router-link>
					</li>
					<li v-else>
						<Logout v-on:changeStatus="logout"/>
					</li>
					<li class="nav-pile">|</li>
					<li>关于</li>
				</ul>
			</div>
		</div>
		<hr>
	</div>
</template>

<script>
import Login from "@/authorize/Login"
import Logout from "@/authorize/Logout"
export default{
	name:"myHeader",
	data(){
		return{
			Login: {
				userName: '登录',
				status: 0
			},
			loginStatus: 0
		}
	},
	// computed: {
	// 	loginStatus() {
	// 		if (sessionStorage.getItem('status') == '1') {
	// 			return parseInt(sessionStorage.getItem('status'))
	// 		}
	// 		return 0
	// 	}
	// },
	created(){
		this.getNameFromSession()
	},
	components: {
		Login,
		Logout
	},
	methods: {
		getNameFromSession() {
			if (sessionStorage.getItem('status') == '1') {
				this.loginStatus = parseInt(sessionStorage.getItem('status'))
				this.$store.state.userName = sessionStorage.getItem('userName')
				this.$store.state.loginStatus = sessionStorage.getItem('status')
			} else {
				this.$store.state.userName = '登录',
				this.Login.status = 0
			}	
		},
		login(data) {
			this.loginStatus = data
		},
		logout(data) {
			this.loginStatus = data
			this.$store.dispatch("logout")
		}
	}
}
</script>
<style scoped>

.app-head {
	/*background: #DBDBDB;*/
	/*color: #b2b2b2;*/
	height: 80px;
	line-height: 90px;
	width: 100%;
	min-width: 1400px;
	margin-bottom: 80px;
}
.app-head hr {
	width: 80%;
	margin: 0 auto;
	text-align: center;
}
.app-head-inner {
	width: 1200px;
	margin: 0 auto;
}
.app-head-inner span{
	position: relative;;
	top: -20px;
}
.app-head-inner img {
	width: 300px;
	margin-top: 20px;
}
.motto {
	margin-left: 150px; 
	margin-bottom: 10px;
}
.head-nav {
	float: right;
}
.head-nav ul {
	overflow: hidden;
}
.head-nav li {
	cursor: pointer;
	float: left;
	list-style: none;
}
.nav-pile {
	padding: 0 10px;
}
.head-logo {
	float: left;
}
</style>