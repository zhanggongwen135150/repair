<template>
  <view style="display: none;" />
</template>

<script>
const attrs = [
  'titleIcon',
  'titleIconRadius',
  'subtitleText',
  'subtitleSize',
  'subtitleColor',
  'subtitleOverflow',
  'titleAlign',
  'backgroundImage',
  'backgroundRepeat',
  'blurEffect'
]
export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    titleIcon: {
      type: String,
      default: ''
    },
    titleIconRadius: {
      type: String,
      default: ''
    },
    subtitleText: {
      type: String,
      default: ''
    },
    subtitleSize: {
      type: String,
      default: ''
    },
    subtitleColor: {
      type: String,
      default: ''
    },
    subtitleOverflow: {
      type: String,
      default: ''
    },
    titleAlign: {
      type: String,
      default: ''
    },
    backgroundImage: {
      type: String,
      default: ''
    },
    backgroundRepeat: {
      type: String,
      default: ''
    },
    blurEffect: {
      type: String,
      default: ''
    },
    loading: {
      type: Boolean,
      default: false
    },
    frontColor: {
      type: String,
      default: '#ffffff'
    },
    backgroundColor: {
      type: String,
      default: '#000000'
    },
    colorAnimationDuration: {
      type: Number,
      default: 0
    },
    colorAnimationTimingFunc: {
      type: String,
      default: 'linear'
    }
  },
  created () {
    const pages = getCurrentPages()
    const page = pages[pages.length - 1]
    this.__$page = page
    this.$watch('title', () => {
      this.setNavigationBarTitle()
    })
    this.$watch('loading', () => {
      this.setNavigationBarLoading()
    })
    this.$watch(() => [
      this.frontColor,
      this.backgroundColor,
      this.colorAnimationDuration,
      this.colorAnimationTimingFunc
    ],
    () => {
      this.setNavigationBarColor()
    })
    // #ifdef APP-PLUS
    this.__$webview = page.$getAppWebview()
    attrs.forEach(key => {
      const titleNView = {}
      if (this[key] || this[key].length > 0) {
        titleNView[key] = this[key]
      }
      this.setTitleNView(titleNView)
      this.$watch(key, (val) => {
        const titleStyle = {}
        titleStyle[key] = val
        this.setTitleNView(titleStyle)
      })
    })
    // #endif
  },
  beforeMount () {
    this.title && this.setNavigationBarTitle()
    this.setNavigationBarLoading()
    this.setNavigationBarColor()
  },
  methods: {
    setNavigationBarTitle () {
      uni.setNavigationBarTitle({
        // #ifndef MP
        __page__: this.__$page,
        // #endif
        title: this.title
      })
    },
    setNavigationBarLoading () {
      uni[(this.loading ? 'show' : 'hide') + 'NavigationBarLoading']({
        // #ifndef MP
        __page__: this.__$page
        // #endif
      })
    },
    setNavigationBarColor () {
      uni.setNavigationBarColor({
        // #ifndef MP
        __page__: this.__$page,
        // #endif
        frontColor: this.frontColor,
        backgroundColor: this.backgroundColor,
        animation: {
          duration: this.colorAnimationDuration,
          timingFunc: this.colorAnimationTimingFunc
        }
      })
    },
    setTitleNView (titleNView) {
      const webview = this.__$webview
      const style = webview.getStyle()
      if (style && style.titleNView) {
        webview.setStyle({
          titleNView: titleNView
        })
      }
    }
  }
}
</script>
