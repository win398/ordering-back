<template>
  <div id="app">
    <el-container style="height: 100%; border: 1px solid #eee">
      <el-header>
        <div class="title">新闻系统后台</div>
        
        <div class="name">
          <el-avatar :size="25" :src="circleUrl"></el-avatar>
          <span>李子</span>
        </div>
      </el-header>
      <el-container>
        <el-aside width="20%" style="backgroud-color: rgb(238, 241, 246)">
          <el-menu
            :default-openeds="['1', '2', '3']"
            :router="router"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b"
          >
            <el-submenu index="1">
              <template slot="title">
                <i class="el-icon-message"></i>
                <span>新闻管理</span>
              </template>
              <el-menu-item-group>
                <el-menu-item index="/NewsAdd">添加新闻</el-menu-item>
                <el-menu-item index="/">新闻列表</el-menu-item>
              </el-menu-item-group>
            </el-submenu>
            <el-submenu index="2">
              <template slot="title">
                <i class="el-icon-message"></i>
                <span>分类管理</span>
              </template>
              <el-menu-item-group>
                <el-menu-item index="2-1">添加分类</el-menu-item>
                <el-menu-item index="2-2">分类列表</el-menu-item>
              </el-menu-item-group>
            </el-submenu>
            <el-submenu index="3">
              <template slot="title">
                <i class="el-icon-message"></i>
                <span>系统管理</span>
              </template>
              <el-menu-item-group>
                <el-menu-item index="3-1">用户列表</el-menu-item>
                <el-menu-item index="3-2">评论列表</el-menu-item>
              </el-menu-item-group>
            </el-submenu>
          </el-menu>
        </el-aside>
        <el-main>
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      circleUrl: "https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png",
      router: true,
    };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.el-header {
  background-color:#6a737c;
  color: #fff;
  line-height: 60px;
  display: flex;
  justify-content: space-between;
}
.title{
  font-size: 18px;
}
.name{
  font-size: 14px;
}
.el-aside {
  color: #333;
}
</style>
