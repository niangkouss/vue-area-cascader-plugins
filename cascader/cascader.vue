<template>
	<div class="area-wrap" @mouseenter="toSelect" @mouseleave="leave" :ref="tag">
		<div class="text-wrap" ref="textWrap">
			<span class="text">{{area}}</span>
			<i class="arrow"></i>
		</div>
		<div class="content-wrapper" v-show="isHover">
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
			/*	areaTree:[
					{
						"label":"北京",
						"value":1,
						"children":[
							{
								"label":"北京12",
								"value":12,
								"children":[
									{
										"label":"北京121",
										"value":121
									},
									{
										"label":"北京1221北京121",
										"value":121
									},
									{
										"label":"北京12京121",
										"value":121
									},
									{
										"label":"北12",
										"value":122
									},
									{
										"label":"北12",
										"value":122
									},
									{
										"label":"北京123",
										"value":123
									}
								]
							},
							{
								"label":"北京13",
								"value":13,
								"children":[
									{
										"label":"北京131",
										"value":387878781
									},
									{
										"label":"北京132",
										"value":32
									},
									{
										"label":"北京133",
										"value":33
									}
								]
							},
							{
								"label":"北京14",
								"value":1456563
							}
						]
					},
					{
						"label":"广东",
						"value":1,
						"children":[
							{
								"label":"广东12",
								"value":12,
								"children":[
									{
										"label":"广东121",
										"value":121
									},
									{
										"label":"广东122",
										"value":122
									},
									{
										"label":"广东123",
										"value":123
									}
								]
							},
							{
								"label":"广东13",
								"value":13,
								"children":[
									{
										"label":"广东131",
										"value":387878781
									},
									{
										"label":"广东132",
										"value":32
									},
									{
										"label":"广东133",
										"value":33
									}
								]
							},
							{
								"label":"广东14",
								"value":1456563
							}
						]
					},
					{
						"label":"上海",
						"value":1,
						"children":[
							{
								"label":"上海12",
								"value":12,
								"children":[
									{
										"label":"上海121",
										"value":121
									},
									{
										"label":"上海122",
										"value":122
									},
									{
										"label":"上海123",
										"value":123
									}
								]
							},
							{
								"label":"上海13",
								"value":13,
								"children":[
									{
										"label":"上海131",
										"value":387878781
									},
									{
										"label":"上海132",
										"value":32
									},
									{
										"label":"上海133",
										"value":33
									}
								]
							},
							{
								"label":"上海14",
								"value":1456563
							}
						]
					}
				],*/
				listTree:[

				],
				area:this.value,
				listIndex:0,
				resultVal:"",
				wrapHeight:0,
				hoverHeight:0
			}
		},
		beforeCreate() {
		},
		created() {
			this.tag = "area"+Math.random();
		},
		beforeMount() {
		},
		mounted() {
			this.listTree[0] = this.datas;
			let wrapper = this.$refs[this.tag];
			this.wrapHeight = $(wrapper).height();
			this.hoverHeight = this.wrapHeight+1;
			$(wrapper).children(".content-wrapper").css("top",this.wrapHeight);
			console.log($(wrapper).children(".text"));
			$(wrapper).find(".text").css("line-height",this.wrapHeight+"px");
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
				this.isHover = true;
				this.borerBottomColor = $(this.$refs.textWrap).css("border-bottom");
				$(this.$refs.textWrap).css("border-bottom",0);
				$(this.$refs.textWrap).css("height",this.hoverHeight);
			},
			leave(){
				this.isHover = false;
				$(this.$refs.textWrap).css("border-bottom",this.borerBottomColor);
				$(this.$refs.textWrap).css("height",this.wrapHeight);
			},
			toClose(){
				this.isHover = false;
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
					//this.tabAry[indexAry] = obj;
					Vue.set(this.tabAry,indexAry,obj);
				}else{
					this.tabAry.push(obj);
				}
				this.selectCity(indexAry);
				/*替换tab,加红框*/
				this.handleChildren(indexAry,index);
			},
			handleChildren(indexAry,index){
				let hasChild = this.listTree[indexAry+1];
				let child = this.listTree[indexAry][index].children;
				if(hasChild){
					this.listTree.splice(indexAry+1);//$remove(item)
					this.tabAry.splice(indexAry+1);//$remove(item)
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
			}
		},
		filters: {},
		computed: {

		},
		watch: {
			value(val) {
				this.area = val;
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
				background:  url("./img/ico.png") no-repeat 3px -124px;
				cursor: pointer;
			}
			.area-tab{
				cursor: default;
				width: 100%;
				height: 25px;
				border-bottom: 2px solid #e4393c;
				overflow: visible;
				a{
					&.cur{
						height: 25px;
						background-color: #fff;
						border: 2px solid #e4393c;
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
						background: url(//img12.360buyimg.com/uba/jfs/t4660/73/4024943067/1159/f1a90f68/59088392N8b6279bb.png) no-repeat 0 5px;
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

















