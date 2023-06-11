<template>
  <div id="app">
    <Page :pageinfo.sync="pageinfo" @getpageinfo="getchileervalue"></Page>1
  </div>
</template>

<script>
	import Page from './components/page'
export default {
  name: 'app',
  data () {
    return {
		pageinfo:{
		  			index:1,
		  			pagesize:10,
		  			total:'100'
		  		},
    }
  },
  components:{
      Page
  },
  methods:{
  	getchileervalue(data){
  		console.log(data)
  	}
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}

</style>
