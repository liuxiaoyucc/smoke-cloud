<template>
	<view class="page">
		<cigarette ref="cigarette"></cigarette>
		
		<view style="display: flex;flex-direction: row;margin-top: 50rpx;">
			<button @click="fire">点火</button>
			<button @click="flick">弹烟灰</button>
			<button @click="absort">嘬一口</button>
			<button @click="extinguish">掐灭</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			fire() {
				this.$refs.cigarette.fire()
			},
			flick() {
				this.$refs.cigarette.flick()
			},
			absort() {
				this.$refs.cigarette.absort()
			},
			extinguish() {
				this.$refs.cigarette.extinguish()
			}
		}
	}
</script>

<style>
	.page {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
	}

</style>
