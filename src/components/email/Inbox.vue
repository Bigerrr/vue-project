<script setup>
import { getCurrentInstance, ref, onMounted} from 'vue';
import {
  Check,
  Delete,
  Edit,
  Message,
  MessageBox,
  Search,
  Star,
} from '@element-plus/icons-vue'

let { proxy } = getCurrentInstance(); // 相当于选项式api的this

const pageSize = ref(10)
const mailsList = ref([])
const subMailsList = ref([])
const multipleSelection = ref([])
const tableLayout = ref('fixed')

//methods
const handleSelectionChange = (val) => {
  multipleSelection.value = val
}
// 根据read已读状态，调整图标
const readIcon = (read) => {
    return read ? MessageBox : Message
}

const setRead = (index) => {
    mailsList.value[index].hasRead = !mailsList.value[index].hasRead
}

const tableRowClassName = ({
  row,
  rowIndex,
}) => {
  if (mailsList.value[rowIndex].hasRead === false) {
    return 'warning-row'
  }
}

function doPagination(curPage) {
    let start = (curPage - 1) * pageSize.value
    let end = curPage * pageSize.value
    subMailsList.value = mailsList.value.slice(start, end)
}

function deleteMail(index) {
    mailsList.value.splice(index, 1)
}

function getMails() {
    proxy.$axios.get("https://mock.apifox.cn/m1/1117500-0-default/email/1", {
    }).then(function(res){
        mailsList.value = res.data.mailsList
        doPagination(1)
    })
}

getMails() // created: xxx

// onMounted(() => { 同步问题？？？
//     doPagination(1)
// })
</script>

<template>
    <el-row>
        <el-col :span="6">
            <el-button type="danger" @click="deleteMail">删除</el-button>
            <el-button type="danger" @click="getMails">获取</el-button>
        </el-col>
    </el-row>
    <el-table 
    :data="subMailsList"
    :row-class-name="tableRowClassName"
    :table-layout="tableLayout"
    style="width: 100%" 
    height="500"
    @selection-change="handleSelectionChange"
    >
        <el-table-column type="expand">
            <template #default="props">
                <div m="4">
                    <p m="t-0 b-2">Message: {{ props.row.message }}</p>
                </div>
            </template>
        </el-table-column> 
        <el-table-column type="selection" width="55" />
        <el-table-column label="主题" width="230">
            <template #default="scope">
                <span :class="{hasRead: !scope.row.hasRead}">{{ scope.row.title }}</span>
            </template>
        </el-table-column>
        <el-table-column label="发件人">
            <template #default="scope">
                <span :class="{hasRead: !scope.row.hasRead}">{{ scope.row.fromUserName }}</span>
            </template>
        </el-table-column>
        <el-table-column prop="time" label="时间" sortable/>
        <el-table-column label="操作" align="right">
            <template #default="scope">
                <!-- <el-switch v-model="scope.row.hasRead" /> -->
                <el-button type="info" :icon="readIcon(scope.row.hasRead)" circle @click="setRead(scope.$index)"/>
                <el-button type="danger" :icon="Delete" circle @click="deleteMail(scope.$index)"/>
            </template>
        </el-table-column>
    </el-table>
    <el-pagination background :page-size="pageSize" layout="prev, pager, next" :total="mailsList.length" @current-change="doPagination"/>
</template>

<style>
    .hasRead {
        font-weight: bold;
    }

    .el-table .warning-row {
        --el-table-tr-bg-color: var(--el-color-warning-light-9);
    }


</style>