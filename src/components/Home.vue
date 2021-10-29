<template>
  <el-container class="home_container">
  <el-header>
    <div>
      <img src="../assets/logo.png" alt="">
      <span>电商后台管理系统</span>
    </div>
     <el-button type="info" @click="logout">退出</el-button></el-header>
  <el-container>
    <el-aside :width="iscollapase? '64px':'200px'">
  <el-menu
      default-active="2"
      class="el-menu-vertical-demo"
       background-color="#333744"
       text-color="#ffffff"
       :unique-opened="true"
       :collapse="iscollapase"
       :collapse-transition='false'
       :router="true"
       >
       <div class="toggle-button" @click="toggleCollapase">|||</div>
       <!-- 一级菜单 -->
      <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
        <template slot="title">
          <i :class="iconsobj[item.id]" class="iconfont"></i>
          <span>{{item.authName}}</span>
        </template>
        <!-- 二级菜单 -->
          <el-menu-item :index="'/'+item1.path" v-for="item1 in item.children" :key="item1.id">
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>{{item1.authName}}</span>
            </template>
          </el-menu-item>
      </el-submenu>
    </el-menu>
    </el-aside>
    <el-main>
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {
  created () {
    this.getMenuList()
  },
  data () {
    return {
      menuList: [],
      iconsobj: {
        /* 一级菜单的id 对应icon图标 */
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      iscollapase: false
    }
  },
  methods: {
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    // 退出
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    toggleCollapase () {
      this.iscollapase = !this.iscollapase
    }
  }

}
</script>

<style lang='less' scoped>

   .home_container {
        height: 100vh;
    }
    .el-header {
        background-color: #373d41;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-left: 0;
        font-size: 20px;
        color: #fff;
        >div{
            display: flex;
            align-items: center;
            span{
              margin-left:15px ;
            }
        }
        img{
            width: 25px;
            height: 25px;
        };
    }
    .el-aside{
        background-color: #333744;
        .el-menu{
          border-right:none ;
        }
    }
    .el-main {
        background-color: #eaedf1;
    }
    .iconfont{
      margin-right: 15px;
    }
    .toggle-button{
      background-color:#4a5064 ;
      color: #fff;
      line-height: 24px;
      text-align: center;
      cursor: pointer;
      letter-spacing: 0.2em;
    }

</style>
