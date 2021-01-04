<template>
  <div class="app-container">
    <!-- <el-row :gutter="20">
      <el-col :span="6" v-for="(item,index) in list" :key="index" style="margin-top:10px;">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>{{item.title}}</span><span class="time">post on: {{item.postdate}} </span>
          </div>
          <div class="component-item" style="height:320px;">
            <div>{{item.content}}</div>
          </div>
          <div class="flex">
            <el-button icon="el-icon-edit" type="primary" @click.prevent.stop="guide">
              编辑
            </el-button> 
            <el-button icon="el-icon-delete" type="primary" @click.prevent.stop="guide">
              删除
            </el-button> 
          </div>
        </el-card>
      </el-col>
    </el-row> -->
        <el-row :gutter="20">
      <el-col :span="6" v-for="(item,index) in list" :key="index" style="margin-top:10px;">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>{{item.author}}</span><span class="time">post on: {{item.display_time}} </span>
          </div>
          <div class="component-item" style="height:320px;">
            <div v-html="item.content"></div>
          </div>
          <div class="flex">
            <el-button icon="el-icon-edit" type="primary" @click.prevent.stop="guide">
              编辑
            </el-button> 
            <el-button icon="el-icon-delete" type="primary" @click.prevent.stop="guide">
              删除
            </el-button> 
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { fetchList } from '@/api/article'
export default {
  name: 'Notice',
  data() {
    return {
      list:[],
      listQuery:{
        page:1,
        limit:10,
      },
      isPullDown:true,
    }
  },
  mounted() {
    this.getList()
    window.addEventListener('scroll',this.Debounce(this.scroll,200),false)
  },
  methods: {
    scroll(){
      let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
      let clientHeight = document.documentElement.clientHeight || document.body.clientHeight;
      let scrollHeight = document.documentElement.scrollHeight || document.body.scrollHeight;
      if (scrollTop + clientHeight == scrollHeight && this.isPullDown == true) {
        this.listQuery.page = this.listQuery.page + 1
        this.getList();
      }
    },
    getList(){
       fetchList({
         page:this.listQuery.page,
         limit:this.listQuery.limit
       }).then(res=>{
         let result=res.data.items;
         if(res.data.items.length<this.limit){
            this.isPullDown=false;
            this.isFooter=false
            this.list=this.list.concat(result)
         }else{
           if(this.listQuery.page>1 && this.isPullDown==true){
             this.list=this.list.concat(result)
           }else if(this.listQuery.page==1){
             this.list=this.list.concat(result)
           }
         }     
       })

      // this.list=[
      //   {id:1, title:'第一次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'},
      //   {id:2, title:'第二次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'},
      //   {id:3, title:'第一次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'},
      //   {id:4, title:'第二次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'},
      //   {id:3, title:'第一次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'},
      //   {id:4, title:'第二次通知', postdate:'2020-10-1 10:20:12',content:'入党申请的通信请注意...党申请的通信请注意党申请的通信请注意党申请的通信请注意.....'}
      // ]
    },
    Debounce(fn, t){
    let delay = t || 500;
    let timer;
      return function () {
          let args = arguments;
          if(timer){
              clearTimeout(timer);
          }
          timer = setTimeout(() => {
              timer = null;
              fn.apply(this, args);
          }, delay);
      }
    }

  }
}
</script>
<style scoped>
  .flex{
    display: flex;
    justify-content: flex-end;
  }

  .time{
    font-size: 12px;
    color: rgb(200,200,200);
    margin-left:10px;
  }
</style>