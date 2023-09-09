<script setup>
import { ref } from 'vue'
import { Edit, Delete } from '@element-plus/icons-vue'
import { artGetChannelsService, artDelChannelService } from '@/api/article'
import PageCountainer from '@/components/PageContainer.vue'
import ChannelEdit from './components/ChannelEdit.vue'
const channelList = ref([])
const loading = ref(false)
const dialog = ref()

const getChannelList = () => {
  loading.value = true
  artGetChannelsService()
    .then((res) => {
      channelList.value = res.data.data
      loading.value = false
    })
    .catch(() => {})
}
getChannelList() // 在created阶段渲染页面

// 点击编辑按钮
const onEditChannel = (row) => {
  dialog.value.open(row)
}
// 点击删除按钮
const onDelChannel = async (row) => {
  await ElMessageBox.confirm('你确认要删除该分类么', '温馨提示', {
    type: 'warning',
    confirmButtonText: '确认',
    cancelButtonText: '取消'
  })
  await artDelChannelService(row.id)
  ElMessage.success('删除成功')
  getChannelList()
}
// 点击添加按钮
const onAddChannel = () => {
  dialog.value.open({})
}
// 监听弹层子组件，数据更新后重新渲染页面
const onSuccess = () => {
  getChannelList()
}
</script>

<template>
  <PageCountainer title="文章分类">
    <template #extra>
      <el-button @click="onAddChannel">添加分类</el-button>
    </template>
    <el-table v-loading="loading" :data="channelList" style="width: 100%">
      <el-table-column type="index" label="序号" width="100"></el-table-column>
      <el-table-column prop="cate_name" label="分类名称"></el-table-column>
      <el-table-column prop="cate_alias" label="分类别名"></el-table-column>
      <el-table-column label="操作" width="150">
        <!-- row 就是 channelList 的一项， $index 下标 -->
        <template #default="scope">
          <el-button
            :icon="Edit"
            circle
            plain
            type="primary"
            @click="onEditChannel(scope.row, scope.$index)"
          ></el-button>
          <el-button
            :icon="Delete"
            circle
            plain
            type="danger"
            @click="onDelChannel(scope.row, scope.$index)"
          ></el-button>
        </template>
      </el-table-column>
      <!-- 对空数据的处理 -->
      <template #empty>
        <el-empty description="没有数据"></el-empty>
      </template>
    </el-table>

    <ChannelEdit ref="dialog" @success="onSuccess"></ChannelEdit>
  </PageCountainer>
</template>

<style lang="scss" scoped></style>
