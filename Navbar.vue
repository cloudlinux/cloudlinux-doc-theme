<template>
  <header class="navbar">
    <SidebarButton @toggle-sidebar="$emit('toggle-sidebar')"/>

    <div class="left-nav">
      <router-link
        :to="$localePath"
        class="home-link logo"
        v-if="$site.themeConfig.logo">
        <img
          :src="$withBase($site.themeConfig.logo)"
          :alt="$siteTitle"
        >
      </router-link>
      <router-link
        :to="$localePath"
        class="home-link"
        v-if="$siteTitle">
      <span
        ref="siteName"
        class="header-title"
        :class.can-hide="$site.themeConfig.logo"
      >{{ $siteTitle }}</span>
      </router-link>

      <!-- <div class="line"></div> -->

      <AlgoliaSearchBox
        v-if="isAlgoliaSearch"
        :options="algolia"
      />
      <SearchBox v-else-if="$site.themeConfig.search !== false"/>
    </div>
    <div class="right-nav">
      <a :href="$site.themeConfig.try_free" target="_blank" class="btn btn-flat">
        Try Free
      </a>

      <NavLinks class="can-hide"/>
    </div>
  </header>
</template>

<script>
import SidebarButton from './SidebarButton.vue'
import AlgoliaSearchBox from '@AlgoliaSearchBox'
import SearchBox from './SearchBox.vue'
import NavLinks from './NavLinks.vue'

export default {
  components: { SidebarButton, NavLinks, SearchBox, AlgoliaSearchBox },

  data () {
    return {
      linksWrapMaxWidth: null
    }
  },

  mounted () {
    const MOBILE_DESKTOP_BREAKPOINT = 719 // refer to config.styl
    const NAVBAR_VERTICAL_PADDING = parseInt(css(this.$el, 'paddingLeft')) + parseInt(css(this.$el, 'paddingRight'))
    const handleLinksWrapWidth = () => {
      if (document.documentElement.clientWidth < MOBILE_DESKTOP_BREAKPOINT) {
        this.linksWrapMaxWidth = null
      } else {
        this.linksWrapMaxWidth = this.$el.offsetWidth - NAVBAR_VERTICAL_PADDING -
          (this.$refs.siteName && this.$refs.siteName.offsetWidth || 0)
      }
    }
    handleLinksWrapWidth()
    window.addEventListener('resize', handleLinksWrapWidth, false)
  },

  computed: {
    algolia () {
      return this.$themeLocaleConfig.algolia || this.$site.themeConfig.algolia || {}
    },

    isAlgoliaSearch () {
      return this.algolia && this.algolia.apiKey && this.algolia.indexName
    }
  }
}

function css (el, property) {
  // NOTE: Known bug, will return 'auto' if style value is 'auto'
  const win = el.ownerDocument.defaultView
  // null means not to return pseudo styles
  return win.getComputedStyle(el, null)[property]
}
</script>

<style lang="stylus">
  @import './styles/config.styl'

  $navbar-vertical-padding = 0.7rem
  $navbar-horizontal-padding = 1.5rem

  .navbar
    position relative
    display flex
    justify-content space-between
    align-items center
    height 64px
    padding 0 30px 0 41px
    .left-nav, .right-nav
      display flex
      align-items center
    .header-title
      font-size: 16px;
      font-weight: 600;
      color white
      margin-left 26px
    .logo
      width 143px
      height 24px
    .search-box
      border-left 1px solid rgba(255, 255, 255, 0.5)
      margin-left 15px
    @media (max-width: $MQNarrow)
      .search-box
        border-left 0
        margin-left 0
    @media (max-width: $MQMobile)
      .header-title
        display none
      .logo
        width 24px
        height 24px
        overflow hidden

  @media (max-width: $MQMobile)
    .navbar
      padding-left 4rem
      .can-hide
        display none
      .links
        padding-left 1.5rem

  .btn
    // -webkit-appearance: button;
    // -moz-appearance: button;
    // appearance: button;
    padding 0 5px 0 5px

    position: relative;
    background-color: $mainColor;
    border: 1px solid #fff;
    border-radius: 4px;
    font-size: 14px;
    margin 0 1rem 0 1rem
    color: #FFFFFF;
    // padding: 20px;
    // width: 111px;
    height: 43px;
    text-align: center;
    -webkit-transition-duration: 0.4s; /* Safari */
    transition-duration: 0.4s;
    text-decoration: none;
    overflow: hidden;
    cursor: pointer;
    font-weight bold


  .btn:after
    content: "";
    background: #fff;
    display: block;
    position: absolute;
    padding-top: 300%;
    padding-left: 350%;
    margin-left: -20px!important;
    margin-top: -120%;
    opacity: 0;
    transition: all 0.8s


  .btn:active:after
    padding: 0;
    margin: 0;
    opacity: 1;
    transition: 0s
  .btn-flat
    border 2px solid white
    width 111px
    height 43px
    border-radius 4px
    display flex
    align-items center
    justify-content center
</style>
