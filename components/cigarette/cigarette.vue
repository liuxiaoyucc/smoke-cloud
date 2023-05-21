<template>
	<view class="cigarette">
		<!-- 烟灰 -->
		<view class="soot">
			
			<block v-for="(soot_item, s_index) in cigarette.soot_length" :key="s_index">
				<!-- <view class="soot-item" :class="[s_index % 2 ? 'soot-item-one': 'soot-item-two']"> -->
				<view class="soot-item" :class="[s_index == 0 ? (Math.round(Math.random() * 1) == 1 ? 'soot-item-one' : 'soot-item-two') : (Math.round(Math.random() * 1) * s_index / s_index == 1 ? 'soot-item-one' : 'soot-item-two')]">

				</view>
			</block>
			

			
		</view>
		<!-- 烟头火焰, 吸烟动作的时候, 红色部分变亮 -->
		<view class="fire" v-if="cigarette.fire_status">
			<view class="fire-one" style=""></view>
			<view class="fire-two" :style="{height: cigarette.fire_status == 1 ? cigarette.fire_length + 'rpx' : cigarette.fire_length_active + 'rpx'}"></view>
		</view>
		<!-- 烟身火焰下方烧黑的部分, 固定不变, 根据body的长度下移 -->
		<view class="fire-bottom" v-if="cigarette.status != 0">
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
			<view class="fire-bottom-item" style="background-color: #4e4c47;"></view>
		</view>

		<!-- 烟身 -->
		<view class="body" :style="{height: cigarette.length + 'rpx'}">
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
	 * 
	 * 香烟状态
	 * 0未点燃时, 烟灰长度为0, 香烟长度为初始长度, 火焰状态为0, 烟雾状态为0
	 * 1正常燃烧时, 火焰状态为1, 烟雾状态为1
	 * 2吸一口状态, 火焰状态为2, 烟雾状态为2, 烟灰长度变长, 香烟长度减小
	 * 3吸烟结束状态, 火焰状态为0, 烟雾状态为0, 烟灰长度为0
	 * 弹烟灰时, 烟灰长度为0, 火焰状态为1
	 */

	const CIGARETTE_LENGTH = 550 // 香烟初始长度 rpx
	const CIGARETTE_FIRE_LENGTH = 30
	const CIGARETTE_FIRE_LENGTH_ACTIVE = 50
	

	export default {
		name: "cigarette",
		props: {

		},
		data() {
			return {
				timer: '',
				cigarette: {
					length: CIGARETTE_LENGTH, // 香烟长度剩余, 区别于 CIGARETTE_LENGTH
					status: 0, // 香烟状态 0未点燃, 1正常燃烧中, 2吸一口烟状态 3吸烟结束状态
					soot_length: 0, // 烟灰长度, 已4为单位
					smoke_status: 0, // 烟雾状态 0无烟 1正常状态烟雾, 2吸一口状态烟雾
					fire_status: 0, // 火焰状态 0无火, 1正常燃烧, 2 吸一口状态
					fire_length: CIGARETTE_FIRE_LENGTH,
					fire_length_active: CIGARETTE_FIRE_LENGTH_ACTIVE,
				},
			};
		},
		computed: {
			
			soot_color() {
				return Math.round(Math.random() * 1) 
			},
		},
		watch: {
			'cigarette.status'(new_status, old_status) {
				
			},
			'cigarette.fire_status'(new_status, old_status) {
				if (new_status = 0) {
					this.cigarette.soot_length = 0
				}
			}
		},
		onLoad() {
			
		},
		methods: {
			// 点火
			fire() {
				if (this.cigarette.status == 0) { // 未点燃状态才能点火
					this.cigarette.status = 1 // 香烟正常燃烧状态
					this.cigarette.fire_status = 1 // 火焰正常状态
					this.cigarette.smoke_status = 1 // 烟雾正常状态
					this.cigarette.soot_length = 0 // 初始化烟灰长度
					this.cigarette.length = this.cigarette.length - this.cigarette.fire_length - 5 // 减去火焰长度, 减去火焰下方黑边长度
					this.auto_burn()
				}
			},
			// 弹烟灰
			flick() {
				this.cigarette.soot_length = 0 // 烟灰长度清零
			},
			// 吸一口
			absort() {
				if (this.cigarette.status == 1) { // 正常燃烧在状态才能来一口
					this.cigarette.status = 2 // 香烟状态变为吸一口
					this.cigarette.fire_status = 2 // 火焰状态变成吸一口
					this.cigarette.smoke_status = 2 // 烟雾状态变成吸一口
					this.cigarette.soot_length = this.cigarette.soot_length + 12 ; //  烟灰变长一截
					this.cigarette.length = this.cigarette.length - (12 / 4 * 5) - (CIGARETTE_FIRE_LENGTH_ACTIVE - CIGARETTE_FIRE_LENGTH); //  烟体变短
					
					// 恢复香烟状态
					setTimeout(() => {
						this.cigarette.status = 1
						this.cigarette.fire_status = 1
						this.cigarette.smoke_status = 1
						this.cigarette.soot_length = this.cigarette.soot_length + (CIGARETTE_FIRE_LENGTH_ACTIVE - CIGARETTE_FIRE_LENGTH) / 5 * 4
					}, 1500)
				}
				
				
			},
			// 熄烟
			extinguish(is_clear_soot = 1) {
				if (this.cigarette.status != 1) { // 非正常燃烧状态不能熄灭
					return
				}
				clearInterval(this.timer) // 清除定时器
				this.cigarette.status = 3 // 修改香烟状态为结束
				this.cigarette.smoke_status = 0; // 烟雾状态熄灭
				this.cigarette.fire_status = 0 // 熄火
				if (is_clear_soot) {
					this.cigarette.soot_length = 0 // 清除烟灰
				}
				
			},
			/**
			 *  随着时间自燃烧事件
			 * 1. 香烟长度变短
			 * 2. 烟灰长度变长
			 */
			auto_burn() {
				this.timer = setInterval(() => {
					this.cigarette.length -= 5 // 香烟长度随时间变短
					this.cigarette.soot_length += 4 // 烟灰长度随时间变长
					if (this.cigarette.length <= 0) {
						clearInterval(this.timer)
						this.extinguish(0)
					}
				}, 300)
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

	.soot {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
	}
	.soot-item {
		width: 25%;
		height: 5rpx;
	}

	.soot-item-one {
		background-color: #75736f;
	}
	.soot-item-two {
		background-color: #4e4c47;
	}

	.fire {
		display: flex;
		flex-direction: column;
	}

	/* .fire-one {
		background-color: #ffa50d;
		height: 10rpx;
	} */

	.fire-two {
		background-color: #fa5c7c;
		
	}

	.fire-bottom {
		display: flex;
		flex-wrap: wrap;
		flex-direction: row;
	}

	.fire-bottom-item {
		width: 25%;
		height: 5rpx;
	}

	.body {
		display: flex;
		flex-direction: row;
		/* flex: 3; */
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
		
	}

	.fliter-tip-top {
		height: 20rpx;
		width: 100%;
		background-color: #d8c96d;
	}

	.fliter-tip-bottom {
		height: 300rpx;
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
