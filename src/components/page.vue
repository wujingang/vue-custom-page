<template>
	<div style="height: 45px;padding-top: 10px;width: 100%;display: flex;line-height: 30px;">
		<!--分页-->
		<div style="width: 300px;">
			当前数据:{{childerpageinfo.total}}条
			<select name="" v-model="selectvalue"  style="width: 80px;height: 35px;outline: none;background-color: #F6F6F6;border: 1px solid #F6F6F6;text-align: center;margin-left: 20px;padding: 0px 10px;box-sizing: border-box;">
				<option :value="item.value" v-for="(item,index) in options">{{item.label}}</option>
			</select>
		</div>
		<div style="flex: 1;text-align: right;">
			<span :class="[wangqiantype == 1 ? 'types iconstyle':'iconstyle']" @click="zhiqian">
				<span>|<</span>
			</span>
			<span :class="[wangqiantype == 1 ? 'types iconstyle':'iconstyle']" @click="shangyiye">
				<
			</span>
			<!-- <span > -->
				<input type="number" min="1" v-model.number="childerpageinfo.index" @keyup.enter="aa" onKeypress="return (/[\d]/.test(String.fromCharCode(event.keyCode)))" style="display: inline-block;width:60px;height: 35px;line-height: 35px;border:1px solid #CACACA;text-align: center;outline: none;box-sizing: border-box;vertical-align: top;" />
			<!-- </span> -->
			<span :class="[wanghoutype == 1 ? 'types iconstyle':'iconstyle']" @click="xiayiye">
				>
			</span>
			<span :class="[wanghoutype == 1 ? 'types iconstyle':'iconstyle']" @click="zhihou">
				>|
			</span>
		</div>
	</div>
</template>

<script>
export default {
name:'pagelist',
props: ['pageinfo'],
computed:{
  	pagelist: function () { 
  		return 
  			this.pageinfo

  	}
  },
  data(){
  	return{
  		childerpageinfo:{
  			index:'',
	  		pagesize:'',
	  		total:'', 
  		},
  		selectvalue:'',
  		options: [{
          value: 10,
          label: '10'
        },{
          value: 20,
          label: '20'
        },{
          value: 50,
          label: '50'
        }],
		
		wangqiantype:0,
		wanghoutype:0,
  	}
  },
  created(){
  	//给下拉默认值 
	this.selectvalue =  this.options[0]['value']
//	this.selectvalue =  1


  },
  mounted(){
  	//接收父传过来的 当前页 、页条数 、总条数
  	this.childerpageinfo.index = this.pageinfo.index
  	this.childerpageinfo.pagesize = this.pageinfo.pagesize
  	this.childerpageinfo.total = this.pageinfo.total
  },
  methods:{
  	//回到第一页
  	zhiqian(){
		//   console.log(16666)
  		this.childerpageinfo.index = 1
		this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
		this.functionesinfo(this.childerpageinfo.index,this.childerpageinfo.total,this.childerpageinfo.pagesize)
  	},
  	//回到上一页
  	shangyiye(){
  		this.childerpageinfo.index = this.childerpageinfo.index - 1
  		if(this.childerpageinfo.index <= 0 ){
			  this.childerpageinfo.index = 1;
		  }else if(this.childerpageinfo.index > Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize) ){
			this.childerpageinfo.index = 1
		}
  		this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
  		this.functionesinfo(this.childerpageinfo.index,this.childerpageinfo.total,this.childerpageinfo.pagesize)
  	},
  	//下一页
  	xiayiye(){
  		// console.log(3)
			this.childerpageinfo.index = this.childerpageinfo.index + 1
			if(Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize) == 0){
				this.childerpageinfo.index = 1;
			}else if(this.childerpageinfo.index > Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize) ){
				this.childerpageinfo.index = Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize)
			}
			this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
			this.functionesinfo(this.childerpageinfo.index,this.childerpageinfo.total,this.childerpageinfo.pagesize)
  	},
  	//回到最后一页
  	zhihou(){
  		this.childerpageinfo.index = Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize)
  		this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
  		this.functionesinfo(this.childerpageinfo.index,this.childerpageinfo.total,this.childerpageinfo.pagesize)
	  },
	aa(){
		// console.log(this.childerpageinfo.index)
		if(this.childerpageinfo.index > Math.ceil(this.childerpageinfo.total/this.childerpageinfo.pagesize)){
			this.childerpageinfo.index = 1;
			this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
		}else if(this.childerpageinfo.index <= 0){
			this.childerpageinfo.index = 1;
			this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
		}
		else{
		this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
		}
	},
	functionesinfo(index,total,size){
			if(index == 1){
				this.wangqiantype = 1 //设置禁用
			}else{
				this.wangqiantype = 0 //取消设置禁用
			}
			
			if(index ==  Math.ceil(total/size)){
				this.wanghoutype = 1 //设置禁用
			}else{
				this.wanghoutype = 0 //取消设置禁用
			}
	}

  },
  watch:{
		//监听total
		'pageinfo.total'(val, oldVal){
	        this.childerpageinfo.total = val
		},
	  //监听每页条数变化
	  selectvalue(n,o){
	  	this.selectvalue = n
		  this.childerpageinfo.pagesize = this.selectvalue
		  this.childerpageinfo.index = 1;
		  this.$emit('getpageinfo',{index:this.childerpageinfo.index,pagesize:this.childerpageinfo.pagesize,total:this.childerpageinfo.total})
	  },
	  pageinfo: {
	　　　handler(newValue, oldValue) {
		this.childerpageinfo.index = newValue.index
		this.childerpageinfo.total = newValue.total
			this.wangqiantype = 0
			this.wanghoutype = 0
			
			if(newValue.index == 1){
				this.wangqiantype = 1 //设置禁用
			}else{
				this.wangqiantype = 0 //取消设置禁用
			}
			if(newValue.index ==  Math.ceil(newValue.total/newValue.pagesize)){
				this.wanghoutype = 1 //设置禁用
			}else{
				this.wanghoutype = 0 //取消设置禁用
			}
	　　　},
	　　　　deep: true
	　　}
  }
}
</script>

<style scoped>


input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button{
	-webkit-appearance: none !important;
	-moz-appearance: none !important;
	margin: 0;
}

	.types{cursor: no-drop !important;}
	.iconstyle{display: inline-block;width: 35px;height: 35px;line-height: 35px;background-color: #F6F6F6;text-align: center;margin-right: 5px ;}
	.iconstyle:hover{background-color: #e8e8e8;cursor: pointer;}
</style>