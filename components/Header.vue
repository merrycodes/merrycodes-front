<template>
  <div v-fix class="header">
    <div class="header-container">
      <nuxt-link class="logo" :to="'/'">
        <img src="/logo.png" width="26px" height="26px" />
        <span class="text-primary" style="margin-left: 10px">{{ settings.blog_name || 'MerryCodes' }}</span>
      </nuxt-link>
      <ul class="tab link-list">
        <li v-for="(list, index) in links" :key="index" class="tab-item">
          <nuxt-link :to="{ path: list.path }" exact>{{ list.name }}</nuxt-link>
        </li>
      </ul>
    </div>
    <github-corner style="position: absolute; top: 0px; border: 0; right: 0;" />
  </div>
</template>

<script type="text/ecmascript-6">
import _ from 'underscore'
import GithubCorner from './GithubCorner'

const staticMenu = [
  { path: '/', name: '首页' },
  { path: '/category', name: '分类' },
  { path: '/tag', name: '标签' },
  { path: '/archive', name: '归档' }
]
export default {
  components: { GithubCorner },
  directives: {
    fix: {
      inserted(el) {
        let beforeScrollTop = document.documentElement.scrollTop || window.pageYOffset || window.scrollY || document.body.scrollTop
        window.addEventListener(
          'scroll',
          _.throttle(() => {
            const afterScrollTop = document.documentElement.scrollTop || window.pageYOffset || window.scrollY || document.body.scrollTop
            const delta = afterScrollTop - beforeScrollTop
            if (delta === 0) return false
            delta > 0 ? el.classList.add('fixed') : el.classList.remove('fixed')
            setTimeout(() => {
              beforeScrollTop = afterScrollTop
            }, 0)
          }, 200)
        )
      },
      unbind() {
        window.onscroll = null
      }
    }
  },
  data() {
    return {
      links: []
    }
  },
  computed: {
    settings() {
      return this.$store.state.setting.data
    }
  },
  mounted() {
    this.initMenu()
  },
  methods: {
    initMenu() {
      this.links = staticMenu
    }
  }
}
</script>

<style scoped>
a {
  color: #5764c6;
  text-decoration: none;
}

.header {
  position: fixed;
  background-color: rgba(255, 255, 255, 0.98);
  box-shadow: 0 0 3px rgba(14, 14, 14, 0.26);
  border: none;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 200;
  transition: all 0.8s;
}

.fixed {
  transform: translateY(-100%);
}

.header-container {
  max-width: 1200px;
  height: 40px;
  margin: 0 auto;
  padding: 10px 30px;
  font-size: 0.9rem;
  position: relative;
}

.header-container .logo {
  display: flex;
  align-items: center;
  float: left;
  margin: 5px 0;
  font-weight: 600;
  font-size: 1.5em;
}

.header-container .tab {
  list-style: none;
  float: right;
  padding: 0;
  margin: 5px 0;
  border-bottom: none;
}

.header-container .tab .tab-item {
  display: inline-block;
  margin: 0 8px;
}

.header-container .tab .tab-item a {
  border-bottom: 0.1rem solid transparent;
  display: block;
  margin: 0;
  padding: 0.4rem 0.2rem 0.3rem 0.2rem;
  color: #50596c;
  text-decoration: none;
}

.header-container .tab .tab-item a.active {
  border-bottom-color: #5764c6;
  color: #5764c6;
}

@media screen and (max-width: 600px) {
  .link-list {
    display: none;
  }
}

.link-list .tab-item {
  display: inline-block;
  margin: 0 8px;
}

@media screen and (min-width: 600px) {
  .header-menu {
    display: none;
  }
}

.header-container .header-menu-icon {
  display: flex;
  align-items: center;
  float: right;
  height: 40px;
  line-height: 40px;
}

.header-container .header-menu-list {
  max-height: 0;
  position: fixed;
  left: 0;
  top: 52px;
  z-index: 99;
  width: 100%;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.98);
  transition: max-height 0.3s ease;
}

.header-container .open {
  max-height: 768px;
  transition-delay: 0.1s;
}

.header-container .header-menu-list > ul {
  margin: 0;
  padding: 0;
}

.header-container .header-menu-list .header-menu-item {
  position: relative;
  list-style: none;
  text-align: center;
  padding: 10px 0;
}

.header-container .header-menu-list .header-menu-item:after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  border-bottom: 1px solid #e5e5e5;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
}
</style>
