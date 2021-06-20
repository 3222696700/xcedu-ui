<template >
  <div>
    <el-form :model="params">
      系统站点：<el-select v-model="params.siteId" placeholder="请选择站点">
      <el-option
        v-for="item in siteList"
        :key="item.siteId"
        :label="item.siteName"
        :value="item.siteId">
      </el-option>
    </el-select>
      页面昵称：<el-input v-model="params.pageAliase"   style="width: 100px"></el-input>
      <el-button v-on:click="query" type="primary"  size="medium">查询</el-button>
    </el-form>

    <!--页面列表-->
  <el-table
    :data="list"
    stripe
    style="width: 100%">
    <el-table-column
      prop="pageName"
      label="页面名称">
    </el-table-column>
    <el-table-column
      prop="pageAliase"
      label="页面昵称">
    </el-table-column>
    <el-table-column
      prop="pageType"
      label="页面类型">
    </el-table-column>
    <el-table-column
      prop="pageWebPath"
      label="访问路径" >
    </el-table-column>
    <el-table-column
      prop="pagePhysicalPath"
      label="物理路径" >
    </el-table-column>
    <el-table-column
      prop="pageCreateTime"
      label="创建时间" >
    </el-table-column>
  </el-table>
    <!--分页栏-->
    <el-pagination
      background
      layout="prev, pager, next"
      style="float:right"
      :total="total"
      :page-size="params.size"
      :current-page="params.page"
      @current-change="handleCurrentChange"

    >
    </el-pagination>
  </div>
</template>
<script>
  import *  as cmsApi  from '../api/cms'
  export default {
    data() {
      return {
        input: '',
        total: 50,
        params: {
          page: 1,
          size: 10,
          siteId:'',
          pageAliase:''
        },
        list: [],
        siteList:[]
      }
    },
    methods: {
      /*
      *
      * 封装所有查询操作函数
      * */
      query:function () {
          cmsApi.page_list(this.params.page,this.params.size,this.params).then((res)=>{

            //将请求后端数据res中的数据列表赋值给页面变量list
            this.list=res.queryResult.list;
            this.total=res.queryResult.total;

         });
      },
      /*
      * 导航栏页码发生变化时候触发，调用query()函数。
      * */
      handleCurrentChange: function (page) {

        this.params.page=page;
        this.query();
      }
      // ,
      // websit: function () {
      //   cmsApi.websit().then(res=>{
      //     cmsApi.page_list();
      //     this.siteList=res.queryResult.list;
      //   });}
    },
    mounted(){
        this.query();
        this.siteList=[
          {
            siteName:'门户主站',
            siteId:'5a751fab6abb5044e0d19ea1'
          },
          {
            siteName:'SIT测试',
            siteId:'3838438'
          }
          ];
        // this.websit();
    }
  }
</script>
<style>
  /*编写页面样式，不是必须*/
</style>
