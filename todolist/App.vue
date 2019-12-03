<template>
	<div id="app">
		<div class='header'>
			<b>任务计划列表</b>
		</div>
		<div class='main'>
			<p><input v-model="val" @keydown="addFn" class="serch" type="text" placeholder="  例如: 吃饭睡觉打豆豆, 回车添加任务"><button class="serchBtn" @click="serchFn">搜索</button></p>
			<br>
			<p><span>总任务:{{this.list.length}}个 , 
			</span><span>已完成:{{count}}个</span> 
			<input v-model="unfinishFlag"  type="checkbox" @change="unfinishFn">
			未完成 <input @change="finishFn" v-model="finishFlag" type="checkbox">
			已完成</p>
			<br>
			<h3>任务列表:</h3>
			<ul>
				<li v-for="(item,index) in list" :key='item.id'>
					<div>
						<input v-model="item.checkFlag" @change="checkFlagFn"  type="checkbox">
						{{index+1}}.
						{{item.title}}
					</div>
					<div>
						<button class="del" @click='delFn(item.id)'>删除</button>&nbsp;
						<button @click='flagFn(item)' :class='item.flag?"activer":"active"'>{{item.flag?'已完成':'未完成'}}</button>
					</div>
				</li>
			</ul>
			<p><input v-model="flagAll" @change="flagAllFn" type="checkbox"> 全选</p>
		</div>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				flagAll:false,
				finish:0,
				unfinishFlag:false,
				finishFlag:false,
				val:'',
				list:[
					{title:"向往的生活",flag:false,id:0,checkFlag:false},
					{title:"奔跑吧",flag:false,id:1,checkFlag:false},
					{title:"快乐家族",flag:false,id:2,checkFlag:false},
					{title:"极限挑战",flag:false,id:3,checkFlag:false}
				],
				finishList:[]
			}
		},
		computed:{
			count(){
				return this.list.filter(item=>item.flag).length
			}
		},
		created(){
			this.finishList=[...this.list]
		},
		methods:{
			//添加
			addFn(e){
				if(e.keyCode===13){
					let obj ={title:this.val,flag:false,id:new Date()*1,checkFlag:false}
					this.list.push(obj)
					this.val=''
				}
			},
			//删除
			delFn(id){
				let index = this.list.findIndex(item=>item.id==id)
				this.list.splice(index,1)
			},
			//单选
			checkFlagFn(){
				this.flagAll = this.list.every(item=>item.checkFlag)
			},
			//多选
			flagAllFn(){
				this.list.forEach(item=>item.checkFlag=this.flagAll)
			},
			//未完成
			unfinishFn(e){
				this.unfinishFlag = e.target.checked
				if(this.unfinishFlag){
					this.list = this.finishList.filter(item=>item.flag==false)
				}else{
					this.list=this.finishList
				}
			},
			//已完成
			finishFn(e){
				this.finishFlag=e.target.checked
				if(this.finishFlag){
					this.list=this.finishList.filter(item=>item.flag==true)
				}else{
					this.list=this.finishList
				}
			},
			//切换已未完成
			flagFn(item){
				item.flag=!item.flag
			},
			//搜索
			serchFn(){
				this.list = this.finishList.filter(item=>item.title.includes(this.val))
				
			}
		}
	}
</script>

<style>
	*{
		margin: 0;
		padding: 0;
		list-style: none;
		text-decoration: none;
	}
	html,body,#app{
		width: 100%;
		height: 100px;
	}
	#app{
		display: flex;
		flex-direction: column;
	}
	.header{
		height: 50px;
		line-height: 50px;
		text-align: center;
		background: #F56C6C;
	}
	.header b{
		color:#fff;
	}
	.main{
		flex: 1;
	}
	.main ul li {
		height: 30px;
		line-height: 30px;
		display: flex;
	}
	.main ul li div{
		flex: 1;
	}
	.main p,.main h3,.main ul{
		margin-left: 10px;
	}
	.serch{
		width: 80%;
		height: 35px;
		margin: 10px 0 0 10px;
	}
	.serchBtn{
		width: 40px;
		height: 39px;
		line-height: 35px;
		
	}
	.del{
		width: 40px;
		height: 25px;
		border: none;
		background: #999;
		color:#fff;
	}
	.active{
		width: 50px;
		height: 25px;
		border: none;
		background:#67C23A;
		color:#fff;
	}
	.activer{
		width: 50px;
		height: 25px;
		border: none;
		background: #F56C6C;
		color:#fff;
	}
</style>
