<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  export default {
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true
      },
      data: {
        type: Array,
        default: null
      },
      listenScroll: {  // 让我们的scroll要不要去监听滚动事件 比如像推荐列表的页面，我们是不回去关心他的滚动位置的，所以默认是false
        type: Boolean,
        default: false
      }
    },
    mounted() {
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    methods: {
      _initScroll() {
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: this.click
        })

        if (this.listenScroll) {
          let me = this // 保留vue实例的this
          this.scroll.on('scroll', (pos) => {
            me.$emit('scroll', pos)  // better-scroll中的this， 默认指向的是scroll
          })
        }
      },
      enable() {
        this.scroll && this.scroll.enable()
      },
      disable() {
        this.scroll && this.scroll.disable()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      },
      scrollTo() {  // this.scroll指向better-scroll的实例 this.scroll.scrollTo:better-scroll的scrollTo，这里调用apply的原因是，需要穿参数，所以调用better-scrol的scrollTo里面传参数
        this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
      },
      scrollToElement() {   //  apply的第一个参数保证和better-scroll的上下文环境是一样的
        this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
      }
    },
    watch: { // 为什么要在之类写watch？如果这个refresh需要调用方触发的话，就需要在每个调用方都需要关心数据变化
      data() {
        setTimeout(() => {
          this.refresh()
        }, 20)
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">

</style>
