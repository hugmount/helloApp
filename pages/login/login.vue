<template>
	<view>
		<form @submit="formSubmit" @reset="formReset">
			<view class="uni-padding-wrap uni-common-mt">
				<!-- <view>
					<navigator open-type="navigateBack"/> 
				</view> -->
				
				<view class="uni-form-item uni-column">
					<view class="title">用户: </view>
					<input id="username" type="text" v-model="user.name"/>
				</view>
				
				<view class="uni-form-item uni-column">
					<view class="title">密码: </view>
					<input id="password" type="text" v-model="user.password" />
				</view>
				
				<view class="uni-form-item uni-column">
                    <view class="title">checkbox</view>
                    <checkbox-group name="checkbox">
                        <label>
                            <checkbox value="checkbox1" />选项一
                        </label>
                        <label>
                            <checkbox value="checkbox2" />选项二
                        </label>
                    </checkbox-group>
                </view>
				
				<view>
					<text class="title">--{{nickname}}--</text>
				</view>
				<p>父组件接手到的内容：{{ user.name }}</p>
				<childSucces message="lhm_success_login"  @receiveMsg="getMsg" ></childSucces>
				
				<view class="uni-btn-v">
					<button type="primary" @click="login();">登录</button>
				</view>
				<view class="uni-btn-v">
					<button type="primary" @click="cancel();">取消</button>
				</view>
			</view>
		</form>
	</view>
	
</template>

<script>
	import childSucces from '../../conponents/loginSuccess.vue'
	
	import {
	    mapState,
	    mapMutations
	} from 'vuex';
	
	export default {
		data () {
			return {
				user : {
					name : '',
					password : ''
				}
			}
		}
		
		,components: {
			childSucces : childSucces
		}
		
		,computed:{
			...mapState(['nickname'])
		}
		
		,methods : {
			getMsg (msg) {
				console.info('接受到数据: ' ,msg);
				this.user.name = msg
			},
			setName () {
				this.$store.commit('change', '登录成功')
		    },
			
			//登录
			login () {
				console.info('正在登录...');
				uni.showLoading({
					title: '正在登录...',
					duration: 2000,
					success:function(){
						console.info('成功')
						uni.request({
							url: 'http://localhost:8010/training/hjApply/getHjblApplyList', 
							method: 'POST',
							header:{
								'content-type' : 'application/x-www-form-urlencoded'
							},
							success: (res) => {
								console.log(res.data);
							}
						});
					}
				})
				
				//修改store
				this.setName();
			}
			//返回
			,cancel () {
				console.info('返回...');
				uni.navigateBack({
					delta: 1
				});
				console.info('已返回')
			}
		}
		
		,onHide() {
			console.info('页面隐藏');
		}
		//不支持小程序
		,onBackPress(options) {  
			console.info('登录页面点击了返回按钮');
            return true;  
        }
		
	
	}
</script>

<style>
</style>
