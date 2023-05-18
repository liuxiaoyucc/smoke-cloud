<template>
	<view class="cigarette" :style="{height: cigarette.length + 'rpx'}">
		<!-- 烟灰 -->
		<view class="soot"></view>
		<!-- 烟头火焰, 吸烟动作的时候, 红色部分变亮 -->
		<view class="fire">
			<view class="fire-one" style=""></view>
			<view class="fire-two" style=""></view>
		</view>
		<!-- 烟身火焰下方烧黑的部分, 固定不变, 根据body的长度下移 -->
		<view class="fire-bottom">
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #75736f;"></view>
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #75736f;"></view>
			<view class="fire-bottom-item" style="background-color: #75736f;"></view>
			<view class="fire-bottom-item" style="background-color: #75736f;"></view>
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #e5dfcf;"></view>
			<view class="fire-bottom-item" style="background-color: #75736f;"></view>
			<view class="fire-bottom-item" style="background-color: #f7f1df;"></view>
			<view class="fire-bottom-item" style="background-color: #f7f1df;"></view>
			<view class="fire-bottom-item" style="background-color: #e5dfcf;"></view>
		</view>

		<!-- 烟身 -->
		<view class="body">
			<view class="body-left"></view>
			<view class="body-center"></view>
			<view class="body-right"></view>
		</view>
		<!-- 过滤嘴 -->
		<view class="filter-tip">
			<view class="fliter-tip-top"></view>
			<view class="fliter-tip-bottom">
				<view class="fliter-tip-left"></view>
				<view class="fliter-tip-center"></view>
				<view class="fliter-tip-right"></view>
			</view>
			
		</view>
	</view>
</template>

<script>
	/**
	 * 吸烟动作处理
	 * 方案一: 监听麦克风
	 * 方案二: 长按手机屏幕
	 * 弹烟灰动作处理
	 * 方案一: 监听手机背部敲击事件
	 * 方案二: 监听'摇一摇'
	 * 方案三: 连续敲击手机屏幕
	 */
	export default {
		name: "cigarette",
		props: {

		},
		data() {
			return {
				cigarette: {
					length: 850, // 香烟长度, rpx
					status: 0, // 香烟状态 0未点燃, 1正常燃烧中, 2吸一口烟状态 3吸烟结束状态
					soot_length: 0, // 烟灰长度
					smoke_status: 0, // 烟雾状态 0无烟 1正常状态烟雾, 2吸一口状态烟雾
					fire_status: 0, // 火焰状态 0无火, 1正常燃烧, 2 吸一口状态

				},
			};
		},
		watch: {
			
			'cigarette.status'(newValue, oldValue) {
				console.log(newValue)
			}
		},
		methods: {
			// 点火
			fire() {
				console.log('asd')
				this.cigarette.status = 1
				this.cigarette.fire_status = 1
				this.cigarette.smoke_status = 1
			},
			// 弹烟灰
			flick() {
				this.cigarette.soot_length = 0
			},
			// 吸一口
			absort() {
				// 烟灰边长
				this.cigarette.status = 2
			},
			// 熄烟
			extinguish() {
				this.cigarette.status = 3
			},
			// 随着时间自燃烧事件
			auto_burn() {
				this.cigarette.soot_length += 5
				this.cigarette.length -= 5
			}

		},
	}
</script>

<style scoped>
	.cigarette {
		display: flex;
		flex-direction: column;
		width: 80rpx;
	}

	.soot {}

	.fire {
		display: flex;
		flex-direction: column;
	}

	.fire-one {
		background-color: #ffa50d;
		height: 10rpx;
	}

	.fire-two {
		background-color: #fa130f;
		height: 20rpx;
	}
	
	.fire-bottom {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
	}
	.fire-bottom-item {
		width: 25%;
		height: 20rpx;
	}

	.body {
		display: flex;
		flex-direction: row;
		flex: 3;
	}
	.body-left {
		flex: 1.5;
		background-color: #f5efdd;
	}
	.body-center {
		flex: 6;
		background-color: #f7f1df;
	}
	.body-right {
		flex: 2.5;
		background-color: #e5dfcf;
	}

	.filter-tip {
		display: flex;
		flex-direction: column;
		flex: 2;
	}
	.fliter-tip-top {
		height: 20rpx;
		width: 100%;
		background-color: #d8c96d;
	}
	.fliter-tip-bottom {
		flex: 1;
		display: flex;
		flex-direction: row;
	}
	.fliter-tip-left {
		flex: 1.5;
		background-color: #f8a254;
	}
	.fliter-tip-center {
		flex: 6;
		background-color: #f9a75c;
	}
	.fliter-tip-right {
		flex: 2.5;
		background-color: #e59b56;
	}
</style>
