<template>
  <div class="mainNav-wrapper">
    <div class="mainNav-logo">
      <img src="../../assets/logo_s.png" alt="" @click="clickLogo">
      <div class="line"></div>
    </div>

    <div>
      <el-menu
        :default-active="activeIndex"
        class="mainNav-el-menu"
        ref="elmenu"
      >
        <!--不可以用v-if，否则会因为控件加载时序问题导致，指定选中的item失效-->
        <div v-show="this.activeIndex !== '应用概述'">
          <el-menu-item index="应用列表" @click="clickSubItem">
            <i class="icon-ic_applist"></i>
            <span slot="title">应用列表</span>
          </el-menu-item>
          <el-menu-item index="团队管理" @click="clickSubItem">
            <i class="icon-ic_mnggp"></i>
            <span slot="title">团队管理</span>
          </el-menu-item>
        </div>

        <div v-show="this.activeIndex === '应用概述'">
          <el-menu-item index="应用概述" @click="clickSubItem">
            <i class="icon-ic_appdes"></i>
            <span slot="title">应用概述</span>
          </el-menu-item>
          <el-menu-item index="应用设置" @click="clickSubItem">
            <i class="icon-ic_appsetting-copy"></i>
            <span slot="title">应用设置</span>
          </el-menu-item>
        </div>
      </el-menu>
    </div>

    <div class="mainNav-footer">
      <el-menu
        class="mainNav-footer-list"
      >
        <el-menu-item index="API文档" @click="clickSubItem">
          <i class="icon-ic_api_s"></i>
          <span slot="title">API文档</span>
        </el-menu-item>
        <el-menu-item index="Help" @click="clickSubItem">
          <i class="icon-ic_help_s"></i>
          <span slot="title">Help</span>
        </el-menu-item>
        <el-menu-item index="GitHub" @click="clickSubItem">
          <i class="icon-ic_github_s"></i>
          <span slot="title">GitHub</span>
        </el-menu-item>
        <el-menu-item index="About" @click="clickSubItem">
          <i class="icon-ic_about_s"></i>
          <span slot="title">About</span>
        </el-menu-item>
      </el-menu>
    </div>

    <div class="rightLine"></div>
  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    data() {
      return {
        activeIndex: '应用列表'
      }
    },
    mounted() {
      this.bus.$on('appdetail', () => {
        this.activeIndex = '应用概述'
      })
      this.bus.$on('applist', () => {
        if (this.$route.fullPath !== '/apps') {
          this.$router.push('/apps')
        }
        this.activeIndex = '应用列表'
      })
      if (this.$route.fullPath === '/members') {
        this.activeIndex = '团队管理'
      }
    },
    created() {
    },
    destroyed() {
      this.bus.$off('applist')
      this.bus.$off('appdetail')
    },
    methods: {
      clickSubItem(data) {
        if (data.index === '应用列表') {
          this.$router.push('/apps')
        }
        if (data.index === '应用概述') {
          this.bus.$emit('appSummary')
        }
        if (data.index === '应用设置') {
          this.bus.$emit('appSetting')
        }
        if (data.index === '团队管理') {
          this.$router.push('/members')
        }
        if (data.index === 'API文档') {
          let href = `${this.axios.defaults.baseURL}api/swagger`
          window.open(href, '_blank')
        }
        if (data.index === 'Help') {
          var domain = this.axios.defaults.baseURL
            .replace('https://', '')
            .replace('http://', '')
            .replace('www.', '')

          let href = `http://wiki.${domain}`
          window.open(href, '_blank')
        }
        if (data.index === 'GitHub') {
          let herf = 'https://github.com/HeadingMobile'
          window.open(herf, '_blank')
        }
        if (data.index === 'About') {
        }
      },
      gotoApiDoc() {

      },
      clickLogo() {
//        this.$router.push('/apps')
      }
    },
    watch: {
      '$route': (to) => {
      }
    }
  }
</script>

<style lang="scss">
  @import "../../common/scss/base";

  .mainNav-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
  }
  .mainNav-logo {
    width: 100%;
    height: 72px;
    text-align: center;
    border-bottom: solid 1px #D5DFED;
    box-sizing: border-box;
  }
  .mainNav-logo img {
    width: 126px;
    height: 25px;
    margin-top: 22px;
    background-size: 126px 25px;
  }
  .mainNav-logo .line {
    width: 96px;
    height: 5px;
    margin: 17px auto 0px auto;
    background-color: $mainColor;
  }
  .mainNav-footer {
    position: absolute;
    bottom: 30px;
    left: 0px;
    width: 100%;
  }
  .mainNav-footer .mainNav-footer-list {
    border-right-width: 0px;
  }
  .mainNav-footer .mainNav-footer-list .el-menu-item {
    height: 48px;
    line-height: 48px;
  }
  .mainNav-footer .mainNav-footer-list .el-menu-item span {
    color: $subTitleColor;
    font-size: 16px;
    line-height: 24px;
  }
  .mainNav-footer .mainNav-footer-list .is-active span {
    color: white;
  }
  .mainNav-wrapper .mainNav-el-menu {
    margin-top: 28px;
    border-right-width: 0px;
  }
  .mainNav-wrapper .mainNav-el-menu .el-menu-item {
    margin-bottom: 18px;
  }
  .mainNav-wrapper .mainNav-el-menu .el-menu-item span {
    font-size: 18px;
  }
  .mainNav-wrapper .mainNav-el-menu i {
    font-size: 18px;
    line-height: 55px;
    margin-right: 10px;
    margin-left: 20px;
  }
  .mainNav-wrapper .mainNav-el-menu i:before {
  }
  .mainNav-wrapper .mainNav-el-menu .is-active {
    color: white;
    background-color: $mainColor;
  }
  .mainNav-wrapper .mainNav-el-menu .is-active i:before {
    color: white;
  }
  .mainNav-wrapper .mainNav-el-menu .is-active span {
    color: white;
  }
  .mainNav-footer .mainNav-footer-list i {
    margin-left: 20px;
    line-height: 55px;
    font-size: 18px;
    margin-right: 10px;
  }
  .mainNav-footer .mainNav-footer-list .is-active {
    background-color: transparent;
  }
  .mainNav-footer .mainNav-footer-list .is-active span {
    color: $subTitleColor;
  }
  .rightLine {
    position: absolute;
    width: 1px;
    top: 72px;
    right: 0px;
    height: calc(100% - 72px);
    background-color: #D5DFED;
  }
</style>
