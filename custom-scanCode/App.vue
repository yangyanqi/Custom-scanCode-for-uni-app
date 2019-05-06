<script>
	export default {
		onLaunch: function() {
			console.log('App Launch');
			// #ifdef APP-PLUS  
                 if(plus.os.name=='Android'){
					 var main = plus.android.runtimeMainActivity();  
                    var Intent = plus.android.importClass('android.content.Intent');  
                    var Activity = plus.android.importClass('android.app.Activity');  
                    var IntentFilter = plus.android.importClass('android.content.IntentFilter');  
                    var broadcastReceiver = plus.android.importClass('android.content.BroadcastReceiver');
					var context = plus.android.importClass('android.content.Context');
                    var intent = new Intent(main, main.getClass());  
                    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
				 }
           
			// #endif 
		},
		onShow: function() {
			console.log('App Show');
		},
		onHide: function() {
			console.log('App Hide');
		},
		methods: {
			/*
			 * 版本号比较方法
			 * 传入两个字符串，当前版本号：curV；比较版本号：reqV
			 * 调用方法举例：compare("1.1","1.2")，将返回false
			 */
			compareVersion(reqV, curV) {
				if (curV && reqV) {
					var arr1 = curV.split('.'),
						arr2 = reqV.split('.');
					var minLength = Math.min(arr1.length, arr2.length),
						position = 0,
						diff = 0;
					while (position < minLength && ((diff = parseInt(arr1[position]) - parseInt(arr2[position])) == 0)) {
						position++;
					}
					diff = (diff != 0) ? diff : (arr1.length - arr2.length);
					return diff < 0;
				} else {
					console.log("版本号不能为空");
					return false;
				}
			},
			download(url) {
				var wgtWaiting = plus.nativeUI.showWaiting("开始下载");
				const downloadTask = uni.downloadFile({
					url: url,
					success: (downloadResult) => {
						if (downloadResult.statusCode === 200) {
							wgtWaiting.setTitle("开始安装");
							plus.runtime.install(downloadResult.tempFilePath, {
								force: false
							}, function() {
								console.log('install success...');
								plus.nativeUI.closeWaiting();
								plus.runtime.restart();
							}, function(e) {
								plus.nativeUI.closeWaiting();
								console.error('install fail...');
							});
						}
					}
				});
				downloadTask.onProgressUpdate((res) => {
					wgtWaiting.setTitle("已下载 " + res.progress + "%");
					if (res.progress >= 100) {
						wgtWaiting.setTitle("开始安装更新...");
					}
				});
			}
		}
	};
</script>

<style>
	/*每个页面公共css */
	@import './common/uni.css';
</style>
