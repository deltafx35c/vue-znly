<template>
	<div class="reviews" v-show="isShow">
		<h4>热门评论</h4>
		<section v-for="(item, index) in reviewData">
			<div class="reviews-header">
				<span class="reviews-author">{{ item.SSR_NAME }}</span>
				评分:&nbsp;<span class="reviews-rate">{{ showRate(item.SSR_POINT) }}</span>
				<span>{{ item.ENTRY_DATE_TIME }}</span></div>
			<div class="reviews-body">
				<p>{{ item.SSR_CONTENT }}</p>
			</div>
		</section>
		<a v-on:click="addReview">新增</a>
		<a>查看更多评论</a>
	</div> 
</template>

<script>
	export default {
		data() {
			return {
				isShow: true,
				SS_NO: this.id,
				reviewData: []
			}
		},
		props: ['id'],
		mounted() {
			this.initPage();
		},
		methods: {
			showRate(rate) {
				if(!rate) return rate = 5; 
				return "★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
			},	
			addReview() {
				let url = `/zhan/saveSsr`;
				this.$http.post(url, {
					SSR_NO: '',
					SS_NO: this.$data.SS_NO,
					SSR_CONTENT: "是19日杭州飞高雄，然后垦丁，花莲，九份，台北5月30日出。有同行"
				}).then( (response) => {
					console.log('输出结果' + response);
				}, (response) => {
					console.log('opps Is Error: ' + response);
				})
			},
			initPage() {
				let url = `/zhan/querySRList?id=${this.$data.SS_NO}`;
				this.$http.get(url).then((response) => {
					this.$data.reviewData = response.data.rows;
				}, (response) => {
					console.log('opps Is Error: ' + response);
				})
			}
		}
	}
</script>


<style scoped lang="scss">
	.reviews {
		text-align: center;
		h4 {
			margin: 10px 0;
			padding: 8px 12px;
			text-align: left;
		}
		section {
			background: #fff;
			margin-bottom: 10px;
			overflow: hidden;
			text-align: left;
			border-radius: 2%;
			box-shadow: 0 0 5px 0 rgba(0, 0, 0, .6);
			.reviews-header {
				display: block;
				font-size: 14px;
				padding: 1% 2%;
				height: 20px;
				width: 100%;
				overflow: hidden;
				.reviews-author, .reviews-rate {
					color: #ff9900;
					margin-right:  5%;
				}
			}
			.reviews-body {
				padding: 4px 2%;
				overflow: hidden;
				font-size: 14px;

			}
			&:last-of-type {
				margin-bottom: 30px;
			}
		}
		a {	
			display: inline-block;
			width: 120px;
			height: 40px;
			line-height: 40px;
			color: #f90;
			border: 1px solid #f90;
			border-radius: 10px;
			&:first-of-type {
				margin-right: 10px;
			}
		}
	}
</style>