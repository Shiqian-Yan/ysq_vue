<template>
  <div class="app-container">
    <el-form label-width="120px">
      <el-form-item label="主页背景">
        <el-upload
          :show-file-list="false"
          :on-success="handleAvatarSuccess"
          :before-upload="beforeAvatarUpload"
          :action="BASE_API+'/blogoss/fileoss'"
          class="avatar-uploader">
          <img :src="bannerData.imageUrl">
          <el-button size="small" type="primary">点击上传</el-button>
        </el-upload>
      </el-form-item>
      <el-form-item>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="updateBanner">保存</el-button>
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
      bannerData:{},
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
      this.bannerData={}
    },
    //修改讲师的方法
    updateBanner() {
      UserApi.updateBanner(this.bannerData)
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
    },
    handleAvatarSuccess(res, file) {
      this.$message({
        type: 'success',
        message: '上传成功!'
      });
      this.bannerData.imageUrl = res.data.url
      this.$forceUpdate()
    },
    //上传之前调用的方法
    beforeAvatarUpload(file) {
      const isJPG = file.type === 'image/jpeg'
      const isLt2M = file.size / 1024 / 1024 < 3

      if (!isJPG) {
        this.$message.error('上传头像图片只能是 JPG 格式!')
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!')
      }
      return isJPG && isLt2M
    }

  }
}
</script>
