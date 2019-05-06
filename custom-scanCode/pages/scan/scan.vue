<template>
	<view class="content">
		<view >二维码数据是：{{result}}</view>
          <button class="btn" type="primary" @tap="open">自定义二维码界面</button>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				text: '页面上要显示的内容：这是一个自定义的扫码界面！',
				type:'scan-listener',
				result:''//获取到的二维码内容，根据自己需求处理结果
				
			}
		},

		methods: {
			open() {
				uni.navigateTo({
					url: '../text/text?text=' + this.text + '&type=' + this.type,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onReady() {
			// #ifdef APP-PLUS
			// #endif
		},
		onLoad() {
			// #ifdef APP-PLUS
			this.$eventHub.$on(this.type, function(data) {
				console.log(data);
				this.text = data;
			});
			// #endif
		},
		onBackPress() {
			// #ifdef APP-PLUS
			plus.screen.lockOrientation('portrait-primary')
			// #endif
		}

	}
</script>

<style>
	.content{
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
.btn{
	width: 50%;
}
</style>
