<script setup>
import { getCurrentInstance, ref } from 'vue';
import {
  Check,
  Delete,
  Edit,
  Message,
  MessageBox,
  Search,
  Star,
} from '@element-plus/icons-vue'
import { read } from '@popperjs/core';
let { proxy } = getCurrentInstance(); // 相当于选项式api的this

const mailsList = ref([])
const multipleSelection = ref([])
//methods
const handleSelectionChange = (val) => {
  multipleSelection.value = val
}
// 根据read已读状态，调整图标
const readIcon = (read) => {
    read ? MessageBox : Message
}

function deleteMail(index) {
    mailsList.value.splice(index, 1)
}

function getMails() {
    proxy.$axios.get("https://mock.apifox.cn/m1/1117500-0-default/email/1", {
    }).then(function(res){
        mailsList.value = res.data.mailsList
    })
}

</script>

<template>
    <el-row>
        <el-col :span="6">
            <el-button type="danger" @click="deleteMail">删除</el-button>
            <el-button type="danger" @click="getMails">获取</el-button>
        </el-col>
    </el-row>
    <el-table :data="mailsList" style="width: 100%" @selection-change="handleSelectionChange" table-layout="auto">
        <el-table-column type="selection" width="55" />
        <el-table-column label="主题" width="230">
            <template #default="scope">
                <span :class="{hasRead: scope.row.hasRead}">{{ scope.row.title }}</span>
            </template>
        </el-table-column>
        <el-table-column prop="fromUserName" label="发件人" />
        <el-table-column prop="time" label="时间" width="180" />
        <el-table-column label="编辑未读状态">
            <template #default="scope">
                <el-switch v-model="scope.row.hasRead" />
                <el-button type="info" :icon="readIcon(scope.row.hasRead)" circle />
                &nbsp;
                <el-button type="danger" :icon="Delete" circle @click="deleteMail(scope.$index)"/>
            </template>
        </el-table-column>
    </el-table>
</template>

<style>
    .hasRead {
        font-weight: bold;
    }
</style>