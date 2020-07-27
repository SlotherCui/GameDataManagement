<template xmlns:value="http://www.w3.org/1999/xhtml">
  <div class="header">
    <el-row class="describe">
      <el-col :span="1">
        <img src="/static/logo.png" width="90px" style="margin-top: -30px"/>
      </el-col>
      <el-col :span="2">
        <template >
          <i class="el-icon-upload"></i>
          <span>web数据管理</span>
        </template>
      </el-col>
      <el-col :span="2">
        <template >
        <i class="el-icon-tickets"></i>
        <span>山东大学软件学院</span>
      </template></el-col>
      <el-col :span="1">
        <template>
        <i class="el-icon-star-off"></i>
        <span>崔玉峰</span>
      </template></el-col>
      <el-col :span="2">
        <template>
        <i class="el-icon-service"></i>
        <span>201600301079</span>
      </template></el-col>
    </el-row>
    <el-button class="back" type="primary" size="small" @click="back">退出</el-button>
    <div class="title">游戏数据管理</div>
    <div class="input-group">
      <span class="iconfont search">&#xe632;</span>
      <input class="input-border"
             placeholder="请输入你也要搜索的游戏"
            v-model="search_name"
             @keydown.enter="searchByName"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  data () {
    return {
      search_name: ''
      // Img: require('.../assets/logo.png')
    }
  },
  methods: {
    back () {
      this.$router.push({path: '/'})
    },
    searchByName () {
      this.$emit('search_name', this.search_name) // 传递给父组件
    }
  }
}
</script>

<style lang="stylus" scoped>
  .header
    text-align: center;
    background-color: #35cbab;
    height: 160px
    color: #fff
    font-size 14px
    padding-top 105px
    .describe
      position absolute
      top: 2%
      left 3%
      font-family: Arial
      width 120%
    .back
       position absolute
       top: 1%
       right 5%
    .title
      font-size: 26px
      padding-top: 24px
      font-weight: 400
      margin-bottom: 20px
      margin-top: 0
    .input-group
      display: inline-block;
      width: 650px;
      height: 50px;
      border-radius: 50px;
      background-color: #4bd7c1;
      position relative
      .input-border
        width: 600px;
        height: 36px;
        margin-top: 6px;
        border-radius: 50px;
        color: #535353;
        font-family: sans-serif;
        background-color: #f6f6f6;
        padding-left 36px
        font-size 15px
        border: 1px solid hsla(0,0%,100%,.23);
      .search
        font-size: 20px;
        color: #4bd7c1
        position absolute
        top 50%
        margin-left 12px
        transform: translateY(-50%);
</style>
