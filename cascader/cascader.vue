<template>
	<div class="area-wrap" @mouseenter="toSelect" @mouseleave="leave" :data-tag="tag">
		<div class="text-wrap" ref="textWrap" @click="toSelectClick($event)">
			<span class="text">{{area}}</span>
			<i :class="['arrow',{'open':isHover}]"></i>
		</div>
		<div class="content-wrapper" v-show="isHover" style="display:none;">
			<i class="close" @click="toClose"></i>
			<div class="area-tab">
				<a href="javascript:;" v-for="(item,index) in tabAry" :data-label="item.label" :data-value="item.value" @click="selectCity(index)" :class="{'cur':item.isCur}">
					<span>{{item.label}}</span>
					<i></i>
				</a>
			</div>
			<div class="area-content">
				<!-- <div v-for="(itemAry,indexAry) in listTree" :data-index="indexAry" v-show="indexAry == listIndex" >-->
				<div v-for="(itemAry,indexAry) in listTree" :data-index="indexAry" v-show="indexAry == listIndex">
					<ul class="area-lists">
						<li v-for="(item,index) in itemAry">
							<a href="javascript:;" :data-value="item.value" :data-label="item.label" @click="selectProvince(item.value,item.label,indexAry,index)">
								{{item.label}}
							</a>
						</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	import $ from 'jquery'
	import Vue from 'vue'
	export default {
		name: 'app',
		components: {},
		props: {
			value: {
			},
			datas: {
				type: Array,
				required: true
			},
			trigger:{
				type:String
			}
		},
		data() {
			return {
				tag:"",
				isHover:false,
				borerBottomColor:null,
				tabAry:[
					{
						value:0,
						label:"请选择",
						isCur:true
					}
				],
				resultAry:["请选择"],
				listTree:[

				],
				area:this.value,
				listIndex:0,
				resultVal:"",
				wrapHeight:0,
				hoverHeight:0,
				wrapper:null,
				isClick:this.trigger === 'click',
				tag2:false
			}
		},
		beforeCreate() {
		},
		created() {
		},
		beforeMount() {
			this.tag = "area"+Math.random();
		},
		mounted() {
			this.listTree[0] = this.datas;
			$("body .area-wrap").each( (index,obj)=> {
				obj.dataset.tag == this.tag? this.wrapper = obj:void 0;
			});
			this.wrapHeight = $(this.wrapper).height();
			this.hoverHeight = this.wrapHeight+1;
			$(this.wrapper).children(".content-wrapper").css("top",this.wrapHeight);
			$(this.wrapper).find(".text").css("line-height",this.wrapHeight+"px");
			if(this.isClick){
				$("body").click((e)=>{
					this.tag2 = false;
					let ary = $(e.target).parents();
					ary.each((index)=>{
						if($(ary[index]).hasClass('area-wrap') == true){
							this.tag2 = true;
						}
					});
				});
			}

		},
		beforeUpdate() {
		},
		updated() {
		},
		activited() {
		},
		deactivated() {
		},
		beforeDestroy() {
		},
		destroyed() {
		},
		methods: {
			toSelect(){
				if(this.isClick){
					return;
				}
				this.store();
			},
			toSelectClick(){
				if(!this.isClick){
					return;
				}
				this.store();
			},
			leave(){
				if(this.isClick){
					return;
				}
				this.restore();
			},
			leaveClick(){
				this.restore();
			},
			toClose(){
				this.restore();
			},
			store(){
				this.isHover = true;
				this.borerBottomColor = $(this.wrapper).find(".text-wrap").css("border-bottom");
				$(this.wrapper).find(".text-wrap").css("border-bottom",0);
				$(this.wrapper).find(".text-wrap").css("height",this.hoverHeight);
			},
			restore(){
				this.isHover = false;
				$(this.wrapper).find(".text-wrap").css("border-bottom",this.borerBottomColor);
				$(this.wrapper).find(".text-wrap").css("height",this.wrapHeight);
			},
			selectCity(index){
				this.listIndex = index;
				this.tabAry.forEach((item,ide)=>{
					ide == index?item.isCur = true:item.isCur = false;
				});

			},
			selectProvince(val,label,indexAry,index){
				let isSelected = this.tabAry[indexAry];
				let obj =  {
					value:val,
					label:label,
					isCur:true
				};
				if(isSelected){
					Vue.set(this.tabAry,indexAry,obj);
				}else{
					this.tabAry.push(obj);
				}
				this.selectCity(indexAry);
				this.handleChildren(indexAry,index);
			},
			handleChildren(indexAry,index){
				let hasChild = this.listTree[indexAry+1];
				let child = this.listTree[indexAry][index].children;
				if(hasChild){
					this.listTree.splice(indexAry+1);
					this.tabAry.splice(indexAry+1);
				}
				if(child){
					this.listTree.push(child);
					this.initTabAry(indexAry);
					this.listIndex = indexAry+1;
				}else{
					this.showResult()
					this.listIndex = indexAry;
				}

			},
			initTabAry(indexAry){
				let obj =  {
					value:0,
					label:"请选择",
					isCur:true
				};
				this.tabAry.push(obj);
				this.selectCity(indexAry+1);
			},
			showResult(){
				this.isHover = false;
				this.area = "";
				this.resultVal = "";
				this.tabAry.forEach(item=>{
					this.area += item.label + "/";
					this.resultVal += item.value + "/";
				});
				this.area = this.area.substring(0,this.area.length-1);
				this.resultVal = this.resultVal.substring(0,this.resultVal.length-1);
				this.$emit('change', this.resultVal);
				this.$emit('input', this.area);
				if(this.isClick){
					this.leaveClick();
				}

			}
		},
		filters: {},
		computed: {

		},
		watch: {
			value(val) {
				this.area = val;
			},
			tag2(cur){
				if(!cur){
					this.leaveClick();
				}

			}
		}
	}

</script>

<style lang="scss" scoped>
	.area-wrap{
		padding: 0;
		margin: 0;
		width: 100%;
		height: 100%;
		-webkit-box-sizing: border-box;
		-moz-box-sizing: border-box;
		box-sizing: border-box;
		position: relative;
		z-index: 99999;
		a{
			text-decoration: none;
		}
		.text-wrap{
			min-width: 142px;
			height: 100%;
			background: #fff;
			border: 1px solid #CECBCE;
			padding: 0 24px 0 4px;
			cursor: pointer;
			.text{
				display: block;
				height: 100%;
			}
			.arrow{
				display: block;
				position: absolute;
				top: 50%;
				margin-top: -8.5px;
				right: 4px;
				width: 17px;
				height: 17px;
				background: url("./img/ico.png") no-repeat 3px 5px;
				overflow: hidden;
				transition: .5s;
				&.open{
					transform: rotate(-180deg);
				}
			}
		}
		.content-wrapper{
			/* display: none; */
			position: absolute;
			top: 40px;
			left: 0;
			border: 1px solid #CECBCE;
			width: 390px;
			padding: 12px 12px 15px;
			background: #fff;
			-moz-box-shadow: 0 0 5px #ddd;
			-webkit-box-shadow: 0 0 5px #ddd;
			box-shadow: 0 0 5px #ddd;
			z-index: -2;
			i.close{
				position: absolute;
				z-index: 2;
				top: 12px;
				right: 12px;
				width: 17px;
				height: 17px;
				background:  url("./img/close.svg") no-repeat;
				cursor: pointer;
			}
			.area-tab{
				cursor: default;
				width: 100%;
				height: 25px;
				border-bottom: 2px solid #3380F4;
				overflow: visible;
				a{
					&.cur{
						height: 25px;
						background-color: #fff;
						border: 2px solid #3380F4;
						border-bottom: 0;
						padding: 0 25px 0 6px;
						line-height: 22px;
						text-decoration: none;
					}
					position: relative;
					float: left;
					height: 23px;
					line-height: 23px;
					padding: 0 25px 1px 6px;
					margin-right: 4px;
					border: 1px solid #ddd;
					border-bottom: 0;
					color: #005AA0;
					text-align: center;
					cursor: pointer;
					overflow: hidden;
					span{

					}
					i{
						background: url("./img/ico.png") no-repeat 0 5px;
						position: absolute;
						right: 6px;
						top: 5px;
						display: block;
						width: 13px;
						height: 16px;
						overflow: hidden;
						opacity: .5;
					}
				}
			}
			.area-content{
				.area-lists{
					padding-top: 10px;
					overflow: hidden;
					width: 100%;
					margin-bottom: -5px;
					padding-left: 0;
					li{
						padding: 0 0 0 15px;
						line-height: 22px;
						float: left;
						clear: none;
						list-style: none;
						&.long-area{
							width: 175px;
						}
						a{
							display: block;
							padding: 0 4px;
							color: #005aa0;
						}
					}
				}
			}
		}
	}

</style>

















