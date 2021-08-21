<template>
  <div class="app-container">
    网站列表
    <!-- 表格 -->
    <el-table
      :data="list"
      border
      fit
      highlight-current-row> //自动遍历

      <el-table-column prop="id" label="序号"  width="50" />
      <el-table-column prop="title" label="名字"  />
      <el-table-column prop="icon" label="图片" />
      <el-table-column prop="color" label="颜色" />
      <el-table-column prop="href" label="地址" />
      <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
          <router-link :to="'/social/edit/'+scope.row.id">
            <el-button type="primary" size="mini" icon="el-icon-edit">修改</el-button>
          </router-link>
          <el-button type="danger" size="mini" icon="el-icon-delete" @click="removeDataById(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

  </div>
</template>
<script>

import social from '@/api/social'

export default {
  //写核心代码位置
  // data:{
  // },
  data() { //定义变量和初始值
    return {
      list:null,//查询之后接口返回集合
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
      social.getSocialList()
        .then(response =>{//请求成功
          this.list = response.data.data

        })
    },
    removeDataById(id) {
      this.$confirm('此操作将永久删除讲师记录, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {  //点击确定，执行then方法
        //调用删除的方法
        social.deleteSocial(id)
          .then(response =>{//删除成功
            //提示信息
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            //回到列表页面
            this.getList()
          })
      }) //点击取消，执行catch方法
    }

  }
}
</script>
