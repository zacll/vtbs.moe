<template>
<div id="app" class="has-text-black">
  <div class="top background" v-if="!badgeView"></div>
  <div class="tabs top" v-if="!badgeView">
    <ul>
      <router-link v-for="([text, icon], url) in links" :key="`menu_${url}`" :to="url" tag="li" exact-active-class="is-active"><a class="bigger">{{text}}<span v-if="icon" :class="`el-icon-${icon}`"></span></a></router-link>
    </ul>
  </div>
  <div class="height" v-if="!badgeView"></div>
  <transition name="hide">
    <span class="tag is-primary is-rounded is-medium staticSticky" v-if="fullscreenLoading">正在连接服务器...</span>
  </transition>
  <router-view>
  </router-view>
</div>
</template>

<script>
import 'element-ui/lib/theme-chalk/display.css'

export default {
  name: 'app',
  data() {
    this.links = {
      '/': ['🍉'],
      '/live': ['直播势', 'd-caret'],
      '/rise': ['急上升', 'top'],
      '/drop': ['(急下降)'],
      '/detail': ['详细', 'discover'],
      '/dd': ['DD风云榜', 's-promotion'],
      '/macro': ['VTB宏观', 'zoom-in'],
      '/tietie': ['贴贴'],
      '/about': ['关于', 'document'],
    }
    return {
      fullscreenLoading: true,
    }
  },
  sockets: {
    connect: function() {
      this.fullscreenLoading = !this.$socket.connected
      setTimeout(() => {
        this.fullscreenLoading = !this.$socket.connected
      }, 1000)
    },
    disconnect: function() {
      this.fullscreenLoading = !this.$socket.connected
      setTimeout(() => {
        this.fullscreenLoading = !this.$socket.connected
      }, 1000)
    },
  },
  computed: {
    activeIndex: function() {
      return this.$route.path
    },
    badgeView() {
      return this.$route.path.includes('badge')
    },
  },
  methods: {},
}
</script>

<style scoped>
/* * {
  font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
} */
.top {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 100;
}

.background {
  height: 49px;
  backdrop-filter: blur(6px) grayscale(50%);
  background-color: rgba(255, 255, 255, 0.3);
}

.tabs {
  padding: 8px 20px;
}

.staticSticky {
  position: fixed;
  bottom: 10px;
  right: 10px;
}

.hide-enter-active,
.hide-leave-active {
  transition-timing-function: ease-in;
  transition: opacity 0.5s, right 0.5s;
}

.hide-enter,
.hide-leave-to {
  right: -170px;
  opacity: 0;
}

.height {
  height: 48px;
}
</style>
