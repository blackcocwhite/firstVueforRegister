<template>
  <div id="app">
    <router-view :uid="uid" :wappid="wappid" :url="redirect_url" :fid="fid"></router-view>
  </div>
</template>
<script>

export default {
  name: 'app',
  data(){
    return {
      uid : '',
      wappid: '',
      redirect_url:'http://test.8dsun.com/WebApp/appPersonCenter',
      fid:'',
    }
  },
  mounted() {
    this.uid = this.$route.query.uid;
    this.fid = this.$route.query.fid;
    this.wappid = this.$route.query.wappid;
    this.redirect_url = this.$route.query.redirect_url ? this.$route.query.redirect_url : this.redirect_url;
  },
  components:{
    // todoList,todoForm
  },
  computed:{
    // todoCount(){
    //     return this.todos.length;
    // }
  },
}
</script>

<style>
    html,body,button, input, select, textarea,h1 ,h2, h3, h4, h5, h6 {
      font:normal 100% Microsoft Yahei,'宋体' , Tahoma, Helvetica, Arial, "\5b8b\4f53", sans-serif;
    }
    *{
      padding: 0px;
      margin:0px;
    }
    #app {
      text-align: center;
    }
</style>
