<script setup>
import { ref } from 'vue';
import { Delete } from '@element-plus/icons-vue'
import { ElMessage, ElMessageBox } from 'element-plus'
//data
const toUser = ref('')
const title = ref('')
const message = ref('')

// methods

// function send() {
//     console.log(message.value)
// }

const send = () => {
    ElMessageBox.alert('发送成功', '提示', {
    confirmButtonText: 'OK',
    callback: (action) => {
      ElMessage({
        type: 'info',
        message: `action: ${action}`,
      })
    },
  })
}

function clear() {
    toUser.value = ''
    title.value = ''
    message.value = ''
}

</script>

<template>
    <!-- TODO Change to Element Plus Form-->
    <el-row>
        <el-col :span="12">
            <el-input v-model="toUser">
                <template #prepend>收件人</template>
            </el-input>
        </el-col>
    </el-row>
    <el-row>
        <el-col :span="12">
            <el-input v-model="title" placeholder="无标题">
                <template #prepend>&nbsp&nbsp主题&nbsp&nbsp</template>
            </el-input>
        </el-col>
    </el-row>
    <el-row>
        <el-col :span="12">
            <el-input 
                v-model="message" 
                :rows="10" 
                type="textarea" 
            />
        </el-col>
    </el-row>
    <el-row>
        <el-col :span="6">
            <el-button type="primary" @click="send">
                发送<el-icon class="el-icon--right"><Upload /></el-icon>
            </el-button>
            <el-button type="danger" :icon="Delete" circle @click="clear"/>
        </el-col>
    </el-row>
</template>

<style>
.el-row {
    margin-bottom: 20px;
}

.el-row:last-child {
    margin-bottom: 0;
}

.el-col {
    border-radius: 4px;
}

/* .grid-content {
    border-radius: 4px;
    min-height: 36px;
} */

/* .ep-bg-purple {
    background-color: #d3dce6;
}

.ep-bg-purple-light {
    background-color: #e5e9f2
} */

</style>