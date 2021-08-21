<template>
  <div class="app-container">
    <el-form label-width="120px">
      <el-form-item label="名字">
        <el-input v-model="userData.username"/>
      </el-form-item>
      <el-form-item label="名字下方标语">
        <el-input v-model="userData.slogan" />
      </el-form-item>
      <el-form-item label="notice">
        <el-input v-model="userData.notice" />
      </el-form-item>
      <el-form-item label="头像">
        <el-input v-model="userData.avatar" />
      </el-form-item>
      <el-form-item label="description">
        <el-input v-model="userData.description" />
      </el-form-item>
      <el-form-item label="domian">
        <el-input v-model="userData.domain" />
      </el-form-item>

      <el-form-item>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="updateUser">保存</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>
<script>

import Tinymce from '@/components/Tinymce'
import ImageCropper from '@/components/ImageCropper'
import PanThumb from '@/components/PanThumb'
import UserApi from '@/api/banner/user'
export default {
  components:{ImageCropper,PanThumb,Tinymce},
  data() {
    return {
      userData:{},
      saveBtnDisabled:false , // 保存按钮是否禁用,
      imagecropperShow:false,
      imagecropperKey:0,
      BASE_API:process.env.BASE_API
    }
  },
  created() { //页面渲染之前执行
    this.init()
  },
  watch: {  //监听
    $route(to, from) { //路由变化方式，路由发生变化，方法就会执行
      this.init()
    }
  },
  methods:{
    init(){
      this.userData={}
    },
    //修改讲师的方法
    updateUser() {
      UserApi.updateUser(this.userData)
        .then(response => {
          //提示信息
          this.$message({
            type: 'success',
            message: '修改成功!'
          });
          //回到列表页面 路由跳转
          // this.$router.push({path:'/blog/table'})
          this.init()
        })
    },
    close(){//
      this.imagecropperShow = false
      this.imagecropperKey = this.imagecropperKey + 1;
    }

  }
}
</script>
