<template>
  <div class="footer">
      <!-- <div class="sidebar-width"></div> -->
      <div class="footer-item">
        <a :href="$site.themeConfig.cloudlinuxSite">
          <img src="./we-are-cloudlinux.svg" alt="We are Cloudlinux">
        </a>
      </div>
      <div class="cl-copyright footer-item">2018. CloudLinux Inc</div>
      <div class="footer-links footer-item">
          <div class="footer-link"
               v-for="item in $site.themeConfig.bottomLinks"
               :key="item">
              <a :href="item.url" target="_blank">{{ item.text }}</a>
          </div>
      </div>

      <div class="social footer-item">
        <span class="social-message">Stay in touch</span>
        <a v-for="item in $site.themeConfig.social"
            :key="item"
            :href="item.url" target="_blank">
            <img :src="item.logo">
        </a>
      </div>
  </div>
</template>


<script>
export default {
    name: 'Footer'
}
</script>

<style lang="stylus" scoped>
  @require './styles/config.styl'
  .footer
    margin-left 20px
    margin-right 20px
    color $grayTextColor
    text-align center
    border-top 1px solid $borderColor
    height 103px
    // width 100%
    background #fff
    display flex
    flex-flow row wrap
    justify-content space-around
    align-items center
    // margin-left 2rem
    .footer-item
      height 70px
      display flex
      align-items center
    .cl-copyright
      font-size 12px
      padding 10px
      font-weight 600
      line-height 4
      color #d8d8d8
    .footer-links
      display flex
      .footer-link
        font-size 12px
        font-weight 600
        line-height 4
        color #0075b2
        padding 11px
    img
      cursor pointer

  .social
    display flex
    justify-content center
    align-items center
    padding-left 20px
    font-size 12px
    color #314659
    .social-message
      padding 12px
      position: relative
      &:before
        content ''
        position absolute
        width 1px
        height 19px
        left 0
        top 50%
        transform translate(0, -50%)
        background #f2f4f5
    a
      margin-right 6px
      &:last-child
        margin-right 0


  .sidebar-width
    width $sidebarWidth + 2rem

  @media (max-width: $MQMobile)
    .footer
      justify-content center
      margin-left 0
      div
        display block
</style>
