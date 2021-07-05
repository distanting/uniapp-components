<template>
		<view class="headNum">
			<block>
				<view :class="activeIndex==index? 'item active':'item'" v-for="(item,index) in HeadData" :key="index"
					@click="clickHeadNum(index)">
					<view class="week">{{item.week | filWeek}}</view>
					<view class="sum">{{item.sum | filSum}}</view>
				</view>
			</block>
			<view class="item-mouth" @click="open">
				<view class="mouth">{{headMouth.num}}月</view>
				<view :class="iconDiv? 'mo-ton':'mo-but'"></view>
			</view>

			<uni-calendar ref="calendar" :insert="false" :start-date="'2021-07-02'" :end-date="'2025-10-20'"
				@confirm="confirm" @close="close"></uni-calendar>
		</view>
</template>

<script>
	export default {
		data() {
			return {
				activeIndex: 0,
				HeadData: [
				],
				headMouth: {
					num: 6,
					icon: true
				},
				//开始时间
				dataTime: '',
				//icon
				iconDiv:true,
			};
		},
		methods: {
			initDay(){
				let ininData=new Date()
				this.initDayiTime(ininData);
			},
			clickHeadNum(value) {
				this.activeIndex = value;
				this.$emit('change',this.HeadData[value])
			},
			initDayiTime(value) {
				var sd = require('silly-datetime');
				this.HeadData=[];
				this.dataTime = sd.format(value, 'YYYY-MM-DD');
				this.headMouth.num = this.dataTime.split('-')[1];
				//console.log(this.headMouth.num)
				for (let i = 0; i < 7; i++) {
					let DataNum = this.GetDateStr(this.dataTime, i)
					if (new Date(DataNum).getDay() == 6 || new Date(DataNum).getDay() == 0) {
				
					} else {
						this.HeadData.push({
							week: new Date(DataNum).getDay(),
							sum: DataNum
						})
					}
				}
			},
			open() {
				this.iconDiv=false;
				this.$refs.calendar.open();
			},
			confirm(item) {
				this.dataTime=item.fulldate
				this.initDayiTime(this.dataTime)
				this.iconDiv=true;
			},
			close(){
				this.iconDiv=true;
			},
			GetDateStr(persen, AddDayCount) {

				var dd = new Date(persen);

				dd.setDate(dd.getDate() + AddDayCount); //获取AddDayCount天后的日期

				var y = dd.getFullYear();

				var m = (dd.getMonth() + 1) < 10 ? "0" + (dd.getMonth() + 1) : (dd.getMonth() + 1); //获取当前月份的日期，不足10补0

				var d = dd.getDate() < 10 ? "0" + dd.getDate() : dd.getDate(); //获取当前几号，不足10补0

				return y + "-" + m + "-" + d;

			}
		},
		filters:{
			filSum:function(value){
				return value.split('-')[2]
			},
			filWeek:function(value){
				let week=['星期一','星期二','星期三','星期四','星期五']
				return week[value-1]
			}
		},
		mounted() {
			this.$nextTick(function(){
				let ininData=new Date()
				this.initDayiTime(ininData)
				this.$emit('change',this.HeadData[0]);
			})
		}
	}
</script>

<style lang="scss" scoped>

		.headNum {
			width: 100%;
			height: 96rpx;
			display: flex;
			justify-content: space-evenly;
			background: #FAFAFA;
			.item {
				font-family: PingFang SC, PingFang SC-Regular;
				font-weight: 400;
				text-align: center;
				width: 90rpx;
				box-sizing: border-box;
				border-radius: 16rpx;
				color: #333333;

				.week {
					margin-top: 12rpx;
					font-size: 22rpx;
				}

				.sum {
					font-size: 34rpx;
				}
			}

			.active {
				background: #1D80F8;
				color: #FFFFFF;
			}

			.item-mouth {
				width: 120rpx;
				display: flex;
				align-items: center;
				justify-content: space-around;
				font-size: 40rpx;
				font-weight: 600;

				.mouth {}

				.mo-ton {
					margin-left: -5rpx;
					margin-top: 3rpx;
					display: block;
					width: 0;
					height: 0;
					border-left: 11rpx solid transparent;
					border-right: 11rpx solid transparent;
					border-bottom: 17rpx solid #979797;
				}

				.mo-but {
					margin-left: -5rpx;
					margin-top: 3rpx;
					display: block;
					width: 0;
					height: 0;
					border-left: 11rpx solid transparent;
					border-right: 11rpx solid transparent;
					border-top: 17rpx solid #979797;
				}
			}
		}
</style>
