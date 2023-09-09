<script setup>
import { ref } from 'vue'
import { formatTime } from '@/utils/format.js'
import { baseURL } from '@/utils/request'
// import { artGetDetailService } from '@/api/article'
const visibleDrawer = ref(false)
const imgUrl = ref('')
const formModel = ref({
  title: '', // 标题
  cate_id: '', // 分类id
  cover_img: '', // 封面图片 file 对象
  content: '', // string 内容
  state: '' // 状态
})

const displayDetail = (data) => {
  visibleDrawer.value = true
  formModel.value = data
  imgUrl.value = baseURL + formModel.value.cover_img
  //   console.log(formModel.value)
}

defineExpose({
  displayDetail
})
</script>

<template>
  <el-drawer
    v-model="visibleDrawer"
    title="文章预览"
    direction="rtl"
    size="600px"
  >
    <!-- <el-form :model="formModel">
      <el-form-item label="文章标题" prop="title"> </el-form-item>
    </el-form> -->
    <div>
      <h1>{{ formModel.title }}</h1>
      <div class="info">
        <span>作者：{{ formModel.nickname || formModel.username }}</span>
        <span>&nbsp;&nbsp;发布时间：{{ formatTime(formModel.pub_date) }}</span>
        <span>&nbsp;&nbsp;文章分类：{{ formModel.cate_name }}</span>
        <hr />
      </div>
      <p>
        <img :src="imgUrl" alt="" />
      </p>
      <div class="content" v-html="formModel.content"></div>
    </div>
  </el-drawer>
</template>
<style scoped>
img {
  width: 300px;
  height: 300px;
  display: block;
}
</style>
