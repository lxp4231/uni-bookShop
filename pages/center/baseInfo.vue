<template>
	<view class="u-p-l-20">
		<u-form :model="form" ref="uForm">
			<u-form-item label="昵称" >
				<u-input v-model="form.name" placeholder="请输入昵称" />
			</u-form-item>
			<u-form-item label="修改头像" >
				<oss-upload @upSuccess="upSuccess"></oss-upload>
			</u-form-item>
		</u-form>
		<view class="u-flex u-m-t-20">
			<u-button @click="submit" :ripple="true" type="primary" shape="square" size="medium">提交</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		onReady() {
			// 拿到用户vuex的信息
			this.form.name = this.vuex_user.name
		},
		data() {
			return {
				form: {
					name: '',
					avatar:''
				}
			};
		},
		methods: {
			// 节流
			submit() {
				this.$u.throttle(this.submitNo(), 3000)
			},
			// 拿到子组件上传图片成功事件
			upSuccess(data){
				// 图片上传成功后 保存图片key
				this.form.avatar=data
			},
			// 提交
			async submitNo() {
				// 更新昵称
				if(this.form.name){
					// 准备参数
					const params = {
						name: this.form.name
					}
					// 调用更新用户信息接口
					await this.$u.api.setUserInfo(params)

					// 更新用户信息
					this.$u.utils.setUserInfo()
					this.$u.toast("更新信息成功")
				}
				// 更新头像
				if(this.form.avatar){
					const params={
						avatar:this.form.avatar
					}
					// 发送更改头像请求
					await this.$u.api.authAvatar(params)
					// 更新缓存数据
					this.$u.utils.setUserInfo()
				}
			}
		}
	}
</script>

<style lang="scss">

</style>
