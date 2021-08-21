<template>
  <div class="app-container">
    社交网站表单
    <el-form label-width="120px">
      <el-form-item label="名字">
        <el-input v-model="social.title"/>
      </el-form-item>
      <el-form-item label="图标">
        <el-input v-model="social.icon"/>
      </el-form-item>
      <el-form-item label="颜色">
        <el-input v-model="social.color"/>
      </el-form-item>
      <el-form-item label="网址">
        <el-input v-model="social.href"/>
      </el-form-item>
      <el-form-item>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="saveOrUpdate">保存</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>
<script>
import socialApi from '@/api/social'
import ImageCropper from '@/components/ImageCropper'
import PanThumb from '@/components/PanThumb'
export default {
  components:{ImageCropper,PanThumb},
  data() {
    return {
      social:{},
      saveBtnDisabled:false , // 保存按钮是否禁用,
      imagecropperShow:false,
      imagecropperKey:0,
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
    init() {
      //判断路径有id值,做修改
      if(this.$route.params && this.$route.params.id) {
        //从路径获取id值
        const id = this.$route.params.id
        //调用根据id查询的方法
        this.getInfo(id)
      } else { //路径没有id值，做添加
        //清空表单
        this.teacher = {}
      }
    },
    //根据讲师id查询的方法
    getInfo(id) {
      socialApi.getSocialId(id)
        .then(response => {
          this.social = response.data.data
        })
    },
    saveOrUpdate() {
      //判断修改还是添加
      //根据teacher是否有id
      if(!this.social.id) {
        //添加
        this.saveTeacher()
      } else {
        //修改
        this.updateTeacher()
      }
    },
    //修改讲师的方法
    updateTeacher() {
      socialApi.updateSocial(this.social)
        .then(response => {
          //提示信息
          this.$message({
            type: 'success',
            message: '修改成功!'
          });
          //回到列表页面 路由跳转
          this.$router.push({path:'/social/list'})
        })
    },
    //添加讲师的方法
    saveTeacher() {
      socialApi.addSocial(this.social)
        .then(response => {//添加成功
          //提示信息
          this.$message({
            type: 'success',
            message: '添加成功!'
          });
          //回到列表页面 路由跳转
          this.$router.push({path:'/social/table'})
        })
    },
    cropSuccess(data){
      this.imagecropperShow = false
      this.teacher.avatar = data.url
      this.imagecropperKey= this.imagecropperKey + 1;
    },
    close(){//
      this.imagecropperShow = false
      this.imagecropperKey = this.imagecropperKey + 1;
    }

  }
}
</script>
