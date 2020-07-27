<template>
  <div>
    <el-row class="tac">
      <el-col :span="2"><div><br/></div></el-col>
      <el-col :span="5">
        <ul class="card_list">
          <el-card class="box-card" v-for= "(item,index1) of query" :key="index1">
            <div slot="header"  background-color="#545c64">
              <i class="el-icon-upload" style="color: #25a4bb"></i>
              <span class="clearfix">{{item}}</span>
            </div>
            <el-radio-group  v-model="check_querys[index1]">
              <el-radio class="check"
                        v-for="(item,index) in querys[index1]"
                        :label="item"
                        :key="index+4*index1"
                        @change="radio_change"
              >{{item}}</el-radio>

            </el-radio-group>
              <!--</el-checkbox-group>-->
            <el-input class="my_input"
                      @focus="focusInput(index1)"
                      @change="radio_change"
                      size="mini"
                      v-model="check_querys[index1]"
                      placeholder="请输入内容"></el-input>
          </el-card>
          <el-card class="box-card">
            <div slot="header"  background-color="#545c64">
              <i class="el-icon-upload" style="color: #25a4bb"></i>
              <span class="clearfix">游戏评分和游戏热度</span>
            </div>
            <div>游戏评分区间</div>
            <el-row style="margin-top: 15px">
              <el-col :span="11"><el-input size="mini"  v-model="check_querys[3]" @change="radio_change"></el-input></el-col>
              <el-col :span="2" style="text-align: center"><div>-</div></el-col>
              <el-col :span="11"><el-input  size="mini" v-model="check_querys[4]" @change="radio_change"></el-input></el-col>
            </el-row>
            <div style="margin-top: 15px">游戏人数区间</div>
            <el-row style="margin-top: 15px">
              <el-col :span="11"><el-input  size="mini" v-model="check_querys[5]" @change="radio_change"></el-input></el-col>
              <el-col :span="2" style="text-align: center"><div>-</div></el-col>
              <el-col :span="11"><el-input  size="mini" v-model="check_querys[6]" @change="radio_change"></el-input></el-col>
            </el-row>
          </el-card>
          <el-card class="box-card">
            <div slot="header"  background-color="#545c64">
              <i class="el-icon-upload" style="color: #25a4bb"></i>
              <span class="clearfix">游戏内容</span>
            </div>
            <el-select v-model="datasrc" placeholder="请选择数据源">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
            <el-input class="my_input" placeholder="请输入内容" @change="handleSearchByDesc" v-model="desc"></el-input>
          </el-card>
        </ul>
      </el-col>
      <el-col :span="15">
        <div class="title">游戏列表</div>
        <ul class="list">
          <el-card class="card" v-for="(item,index) of gameData" :key="item._Id">
            <el-row class="item" :gutter="4">
              <el-col :span="5">
                <img class="img-border" referrer="no-referrer|origin|unsafe-url" v-bind:src="item.gameImg"/>
              </el-col>
              <el-col :span="12">
                <div>
                  <span class="text">游戏名称: </span>
                  <span class="value_text">{{item.gameName}}</span>
                </div>
                <div style="margin-top: 15px">
                  <span class="text">英文名称: </span>
                  <span class="value_text">{{item.gameNameEng}}</span>
                </div>
                <div style="margin-top: 15px">
                  <span class="text">游戏类型: </span>
                  <span class="value_text">{{item.gameType}}</span>
                </div>
                <div style="margin-top: 15px">
                  <span class="text">游戏发行时间: </span>
                  <span class="value_text">{{item.gameTime}}</span>
                </div>
                <div style="margin-top: 15px" >
                  <span class="text">游戏制作方： </span>
                  <span class="value_text">{{item.gameAuthor}}</span>
                  <!--<span style="margin-left: 70px" class="text">游戏热度: </span>-->
                  <!--<span style="color: #F56C6C">{{item.gameSumPlayer}}</span>-->
                </div>
                <div style="margin-top: 15px" >
                  <span class="text">游戏热度: </span>
                  <span style="color: #F56C6C" class="value_text">{{item.gameSumPlayer}}</span>
                </div>
              </el-col>
              <el-col :span="7">
                <div class="score">
                  <el-rate
                    v-model="item.gameAvgScore"
                    disabled
                    show-score
                    score-template="{value}">
                  </el-rate>
                  <div style="margin-top: 100px">
                    <el-tag v-if="item.tdmLink" type=success>下载</el-tag>
                    <el-tag v-else  type=info >下载</el-tag>
                    <el-tag v-if="item.steamLink" type=success>购买</el-tag>
                    <el-tag v-else type=info>购买</el-tag>
                    <el-tag v-if="item.youminLink" type=success>攻略</el-tag>
                    <el-tag v-else type=info> 攻略</el-tag>
                  </div>
                </div>
              </el-col>
            </el-row>
            <!--游戏详情-->
            <el-collapse style="margin-left: 15px" @change="getGameDetail">
              <el-collapse-item title="游戏详情" :name="index">

                <!--游民星空!-->
                <div v-if="YouminDataDetail[index]">
                  <a v-bind:href="YouminDataDetail[index].gameLink" target="_blank" style="font-size: 17px">游民星空</a>
                  <el-row class="detailitem" :gutter="4">
                    <el-col :span="5">
                      <img class="img-border" referrer="no-referrer|origin|unsafe-url" v-bind:src="YouminDataDetail[index].gameImg">
                    </el-col>
                    <el-col :span="19">
                      <div>
                        <span class="detailtext">游戏时长：</span>
                        <span>{{YouminDataDetail[index].gamePlayTime}}</span>
                        <br/>
                        <span class="detailtext">游戏标签：</span>
                        <span>{{YouminDataDetail[index].gameTags}}</span>
                        <br/>
                        <span class="detailtext">游戏介绍：</span>
                        <span style="color: #cacaca">{{YouminDataDetail[index].gameDescribe}}</span>
                        <br/>
                        <el-collapse >
                          <el-collapse-item title="游戏攻略：" >
                            <div v-for="item of YouminDataDetail[index].gameStrategys" :key="item.strategy_link">
                              <a v-bind:href="item.strategy_link" target="_blank" >{{item.strategy_name}}</a>
                            </div>
                          </el-collapse-item>
                          <el-collapse-item title="玩家评论：" >
                            <div v-for="(item,indexofcomm) of YouminDataDetail[index].gamePlayerComments" :key="indexofcomm">
                              <i class="el-icon-service" style="font-size: 18px"></i>
                              <span style="color: #cacaca; font-size: 12px">:"{{item}}"</span>
                              <br/>
                            </div>
                          </el-collapse-item>
                        </el-collapse>
                        <!--<span class="detailtext">玩家评论：</span>-->
                        <!--<div v-for="(item,indexofcomm) of YouminDataDetail[index].gamePlayerComments" :key="indexofcomm">-->
                          <!--<i class="el-icon-service" style="font-size: 18px"></i>-->
                          <!--<span style="color: #cacaca; font-size: 12px">:"{{item}}"</span>-->
                          <!--<br/>-->
                        <!--</div>-->
                      </div>
                    </el-col>
                  </el-row>
                </div>
                <!--steam!-->
                <div v-if="SteamDataDetail[index]" >
                  <a v-bind:href="SteamDataDetail[index].gameLink" target="_blank" style="font-size: 17px">Steam游戏平台</a>
                  <el-row class="detailitem" :gutter="4">
                    <el-col :span="5"> <img class="img-border" referrer="no-referrer|origin|unsafe-url" v-bind:src="SteamDataDetail[index].gameImg"></el-col>
                    <el-col :span="19">
                      <div>
                        <span class="detailtext">游戏价格：</span>
                        <span>{{SteamDataDetail[index].gamePrice}}</span>
                        <br/>
                        <span class="detailtext">游戏介绍：</span>
                        <span style="color: #cacaca">{{SteamDataDetail[index].gameDescribe}}</span>
                        <br/>
                        <el-collapse >
                          <el-collapse-item title="关于游戏:">
                            <span style="color: #cacaca; font-size: 13px" >{{SteamDataDetail[index].gameAbout}}</span>
                          </el-collapse-item>
                        </el-collapse>
                      </div>
                    </el-col>
                  </el-row>
                </div>
                <!--3dm游戏网-->
                <div v-if="TdmDataDetail[index]">
                  <a v-bind:href="TdmDataDetail[index].gameLink" target="_blank" style="font-size: 17px">3DM游戏网</a>
                  <el-row class="detailitem" :gutter="4">
                    <el-col :span="5"> <img class="img-border" referrer="no-referrer|origin|unsafe-url" v-bind:src="TdmDataDetail[index].gameImg"></el-col>
                    <el-col :span="19">
                      <div>
                        <span class="detailtext">游戏下载：</span>
                        <a v-bind:href="TdmDataDetail[index].gameDownloader" >下载地址</a>
                        <br/>
                        <span class="detailtext">游戏介绍：</span>
                        <span style="color: #cacaca">{{TdmDataDetail[index].gameDescribe}}</span>
                        <br/>
                        <span class="detailtext">游戏配置:</span>
                        <el-table :data="TdmDataDetail[index].gameConfigure" border>
                          <el-table-column prop="configureName" label="配置项" ></el-table-column>
                          <el-table-column prop="bad" label="最低配置" ></el-table-column>
                          <el-table-column prop="good" label="推荐配置"></el-table-column>
                        </el-table>
                      </div>
                    </el-col>
                  </el-row>
                </div>
              </el-collapse-item>
            </el-collapse>
          </el-card>
        </ul>
      </el-col>

      <el-col :span="2"><div><br/></div></el-col>
    </el-row>
    <el-pagination background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      layout="prev, pager, next"
      :total="page"
      :page-size="10"
      :pager-count="15"
      :current-page="currentPage"
      class="pagination">
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: 'HomeBody',
  props: ['search_name'],
  data () {
    return {
      gameData: [],
      value5: 3.7,
      page: '',
      currentPage: 1,
      query: ['游戏类型', '游戏发行时间', '游戏发行方'],
      querys: [['动作游戏', '角色扮演', '第一人称射击', '即时战略', '策略游戏'],
        ['2019', '2018', '2017', '2016', '2015'],
        ['任天堂', 'CAPCOM', '暴雪', 'Rockstar', '育碧']
      ],
      check_querys: ['', '', '', '0.0', '5.0', '0', '5000000'],
      IsName: true,
      IsDesc: false,
      SteamDataDetail: ['', '', '', '', '', '', '', '', '', ''],
      TdmDataDetail: ['', '', '', '', '', '', '', '', '', ''],
      YouminDataDetail: ['', '', '', '', '', '', '', '', '', ''],
      options: [
        {
          value: 'youmin',
          label: '游民众评'
        },
        {
          value: 'steam',
          label: 'Steam游戏平台'
        },
        {
          value: '3dm',
          label: '3DM游戏网'
        }
      ],
      datasrc: '',
      desc: ''
      // check_querys: [[],[],[],[]]
    }
  },
  methods: {
    getGameInfo (page, searchname) {
      console.log(page)
      this.currentPage = page
      this.IsName = true
      this.IsDesc = false
      this.axios.get('http://localhost:8080/searchByName?name=' + searchname + '&page=' + (page - 1))
        .then(this.getGameInfoSucc)
    },
    getGameInfoSucc (res) {
      // console.log(res)
      res = res.data
      console.log(res)
      if (res.code === 0 && res.data) {
        this.gameData = res.data.content
        this.page = res.data.totalElements
      }
    },
    handleSizeChange (val) {
      console.log(`当前页: ${val}`)
      this.currentPage = val
    },
    // 点击翻页
    handleCurrentChange (val) {
      console.log(`当前页: ${val}`)
      this.currentPage = val
      // 将详情数据清零
      this.SteamDataDetail = ['', '', '', '', '', '', '', '', '', '']
      this.TdmDataDetail = ['', '', '', '', '', '', '', '', '', '']
      this.YouminDataDetail = ['', '', '', '', '', '', '', '', '', '']
      // 获取新一页的数据
      if (this.IsName) {
        this.getGameInfo(val, this.search_name)
      } else if (this.IsDesc) {
        this.getGameByDesc()
      } else {
        this.getGameDataCombine()
      }
      // 回到页面顶部
      this.backTop()
    },
    radio_change () {
      this.currentPage = 1
      this.getGameDataCombine()
    },
    getGameDataCombine () {
      let querys = this.check_querys
      this.IsName = false
      this.IsDesc = false
      console.log(this.currentPage)
      this.axios.post('http://localhost:8080/search', {'querys': querys, 'page': this.currentPage - 1}).then(this.getGameInfoSucc)
    },
    focusInput (i) {
      this.$set(this.check_querys, i, '')
    },
    // 回到顶部
    backTop () {
      let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
      let timer = setInterval(() => {
        let ispeed = Math.floor(-scrollTop / 5)
        console.log(ispeed)
        document.documentElement.scrollTop = document.body.scrollTop = scrollTop + ispeed
        scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
        if (scrollTop === 0) {
          clearInterval(timer)
        }
      }, 16)
    },
    getGameDetail (val) {
      var index = val[0]
      if (index !== undefined) {
        var game = this.gameData[index]
        var links = [game.steamLink, game.tdmLink, game.youminLink]
        this.axios.post('http://localhost:8080/detail', links)
          .then((res) => {
            res = res.data
            if (res.code === 0 && res.data) {
              // this.gameDataDetail[index] = res.data.gameDataSteam
              console.log(res.data)
              // 攻略数据处理
              var json = null
              if (res.data.gameDataYoumin != null) {
                json = res.data.gameDataYoumin.gameStrategys
                var gameStrategys = eval('(' + json + ')')
                res.data.gameDataYoumin.gameStrategys = gameStrategys
                json = res.data.gameDataYoumin.gamePlayerComments
                var gamePlayerComments = eval('(' + json + ')')
                res.data.gameDataYoumin.gamePlayerComments = gamePlayerComments
              }
              // 配置信息处理
              if (res.data.gameDataTdm != null) {
                json = res.data.gameDataTdm.gameConfigure
                var gameConfigure = eval('(' + json + ')')
                console.log(gameConfigure)
                var configure = []
                for (var key in gameConfigure) {
                  for (var key2 in gameConfigure[key]) {
                    var item = gameConfigure[key][key2].split('@')

                    configure.push({'configureName': key2, 'bad': item[0], 'good': item[1]})
                  }
                }
                console.log(configure)
                res.data.gameDataTdm.gameConfigure = configure
              }
              //
              this.$set(this.SteamDataDetail, index, res.data.gameDataSteam)
              this.$set(this.TdmDataDetail, index, res.data.gameDataTdm)
              this.$set(this.YouminDataDetail, index, res.data.gameDataYoumin)
              console.log(this.SteamDataDetail[index])
              console.log(this.TdmDataDetail[index])
              console.log(this.YouminDataDetail[index])
              // this.gameData = res.data.content
              // this.page = res.data.totalElements
              // console.log(this.gameData)
            }
          })
      }
    },
    handleSearchByDesc () {
      this.currentPage = 1
      this.getGameByDesc()
    },
    getGameByDesc () {
      this.IsDesc = true
      this.IsName = false
      this.axios.post('http://localhost:8080/searchByDesc', {'desc': this.desc, 'datasrc': this.datasrc, 'page': this.currentPage - 1}).then(this.getGameInfoSucc)
    }
  },
  mounted () {
    this.getGameInfo(1, '')
  }
}
</script>

<style lang="stylus"  scoped>
.card_list
  margin-left 70px
  margin-top 90px
  .box-card
    margin-top 25px
    .my_input
      margin-top: 10px
    .clearfix
      font-family "Hiragino Sans GB"
      font-size 15px
      font-weight bold
    .check
      AutoSize false
      width 100%
      margin-bottom 7px
.menu
  margin-top 70px
  margin-left 80px

  height 100%
.title
  margin-left 100px
  margin-top 70px
  font-size: 22px;
  font-weight bold
  font-family "Helvetica Neue"
.list
  margin-left 90px
  margin-top 50px
  margin-right 70px
  .card
    margin  15px
    .item
      font-size: 16px;
      border-bottom: 1px dashed #e4e7e9;
      padding 15px
      .img-border
        /*height 85px*/
        width 120px
      .text
        color #535353
        font-family "Helvetica Neue"
        font-weight bold
      .value_text
        color #535353
        font-family "Helvetica Neue"
      .score
        text-align: right
    .detailitem
      font-size: 14px;
      border-bottom: 1px dashed #e4e7e9;
      padding 10px
      .img-border
        width 120px
      .detailtext
        font-family "Helvetica Neue"
        font-weight bold
.pagination
  margin-top 25px
  margin-bottom  45px
  text-align center
  margin-left 170px
</style>
