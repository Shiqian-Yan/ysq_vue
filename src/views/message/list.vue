<template>
  <div class="app-container">
    留言列表
    <!-- 表格 -->
    <el-table
      :data="list"
      border
      fit
      highlight-current-row> //自动遍历

      <el-table-column
        label="序号"
        width="70"
        align="center">
        <template slot-scope="scope">
          {{ (page - 1) * limit + scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column prop="email" label="邮箱" width="200" />
      <el-table-column prop="message" label="留言" />

      <el-table-column label="是否回复" width="80">
        <template slot-scope="scope">
          {{ scope.row.reply===true?'已回复':'未回复' }}
        </template>
      </el-table-column>
      <el-table-column prop="gmtCreate" label="添加时间" width="160"/>
      <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" icon="el-icon-edit" @click="reply(scope.row.id)">修改是否已经回复</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页 -->
    <el-pagination
      :current-page="page"
      :page-size="limit"
      :total="total"
      style="padding: 30px 0; text-align: center;"
      layout="total, prev, pager, next, jumper"
      @current-change="getList"
    />

  </div>
</template>
<script>

import message from '@/api/message'

export default {
  //写核心代码位置
  // data:{
  // },
  data() { //定义变量和初始值
    return {
      list:null,//查询之后接口返回集合
      page:1,//当前页
      limit:10,//每页记录数
      total:0,//总记录数
    }
  },
  created() { //页面渲染之前执行，一般调用methods定义的方法
    //调用
    this.getList()
  },
  methods:{  //创建具体的方法，调用teacher.js定义的方法
    //讲师列表的方法
    getList(page=1) {
      this.page = page
      message.getMessageListPage(this.page,this.limit)
        .then(response =>{//请求成功
          //response接口返回的数据
          //console.log(response)
          this.list = response.data.rows
          this.total = response.data.total
          console.log(this.list)
          console.log(this.total)
        })
    },
    reply(id){
      message.getReplyId(id).then(res=>{
        this.getList()
      })
    }
  }
}
</script>
