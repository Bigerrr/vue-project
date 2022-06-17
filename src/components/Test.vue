<script>
import SendEmail from './email/SendEmail.vue'
export default {
  data() {
    return {
      newTodo: "",
    };
  },
  computed: {},
  methods: {},
  components: {
    SendEmail
  }
}
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-aside width="200px">Aside</el-aside>
      <el-container>
        <el-header>
          <h3>OA系统的首页面</h3>
        </el-header>
        <el-main>
          <SendEmail/>
        </el-main>
        <el-footer>Footer</el-footer>
      </el-container>
    </el-container>
  </div>
</template>

<style>
.el-header {
  line-height: 60px;
  background-color: palegreen;
}

.el-aside {
  background-color: lightblue;
  line-height: 170px;
  outline-width: 5ch;
}

.el-main {
  background-color: white;
}

.el-footer {
  line-height: 100px;
  background-color: salmon;
}
</style>