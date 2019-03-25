<template>
  <div class="ebook">
    <title-bar
      :TitleAndMenuShow="TitleAndMenuShow"/>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleTitleAndMenu"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar
      :TitleAndMenuShow="TitleAndMenuShow"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      :bookAvailabe="bookAvailabe"
      :navigation="navigation"
      @setFontSize="setFontSize"
      @setTheme="setTheme"
      @onProgressChange="onProgressChange"
      @jumpTo="jumpTo"
      ref="menuBar"/>
  </div>
</template>

<script type="text/javascript">
import TitleBar from './TitleBar'
import MenuBar from './MenuBar'
import Epub from 'epubjs'
const DOWNLOAD_URL = '../../static/145083.epub'
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      TitleAndMenuShow: false,
      fontSizeList: [
        { fontSize: 12 },
        { fontSize: 14 },
        { fontSize: 16 },
        { fontSize: 18 },
        { fontSize: 20 },
        { fontSize: 22 },
        { fontSize: 24 }
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000', 'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#000', 'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff', 'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000', 'background': 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0,
      bookAvailabe: false,
      navigation: {}
    }
  },
  methods: {
    jumpTo(href) {
      this.rendition.display(href)
      this.hideTitleAndMenu()
    },
    hideTitleAndMenu() {
      this.TitleAndMenuShow = false
      this.$refs.menuBar.hideSetting()
      // 隐藏目录
      this.$refs.menuBar.hideContent()
    },
    onProgressChange(progress) {
      const percentage = progress / 100
      const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(location)
    },
    setTheme(index) {
      this.themes.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      })
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
    toggleTitleAndMenu() {
      this.TitleAndMenuShow = !this.TitleAndMenuShow
      if (!this.TitleAndMenuShow) {
        this.$refs.menuBar.hideSetting()
      }
    },
    showEpub() {
      this.book = new Epub(DOWNLOAD_URL)
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      this.rendition.display()
      // 获得字体对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.setFontSize(this.defaultFontSize)
      // 注册主题
      this.registerTheme()
      // 设置默认字体
      this.setTheme(this.defaultTheme)
      // 获取location对象 进度条
      this.book.ready.then(() => {
        this.navigation = this.book.navigation
        return this.book.locations.generate()
      }).then(result => {
        this.locations = this.book.locations
        this.bookAvailabe = true
      }).catch(err => {
        console.log(err)
      })
    },
    prevPage() {
      this.TitleAndMenuShow = false
      this.$refs.menuBar.hideSetting()
      // 隐藏目录
      this.$refs.menuBar.hideContent()
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      this.TitleAndMenuShow = false
      this.$refs.menuBar.hideSetting()
      // 隐藏目录
      this.$refs.menuBar.hideContent()
      if (this.rendition) {
        this.rendition.next()
      }
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/global.scss';
.ebook {
  position: relative;
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      display: flex;
      width: 100%;
      height: 100%;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1
      }
      .right {
        flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>
