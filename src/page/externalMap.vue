<template>
	<div>
        <v-header goBack="true" headTitle="外部地图"></v-header>
        <div class="travel-box">
			<div id="allmap"></div>
        </div>

    </div>
</template>

<script>
	import vHeader from '../components/header'
	export default {
		data() {
			return {
			}
		},
		components: {
			vHeader
		},
		mounted() {

			//百度地图API功能
			var map = new BMap.Map("allmap");    // 创建Map实例
			map.centerAndZoom(new BMap.Point(119.192621,34.01304), 20);  // 初始化地图,设置中心点坐标和地图级别
			map.addControl(new BMap.MapTypeControl());   //添加地图类型控件
			map.setCurrentCity("淮安国缘宾馆");          // 设置地图显示的城市 此项是必须设置的
			map.enableScrollWheelZoom(true);     //开启鼠标滚轮缩放
		}
	}
</script>

<style scoped lang="scss">
	.travel-box {
		margin-top: 60px;
	}
	#allmap {
		width: 100%;
		height: 400px;
	}
</style>