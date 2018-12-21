<template>
  <router-link tag="div" class="item-wrapper" :to="path">
    <div class="sidebar-link">
      <i class="material-icons" v-if="links.length" v-on:click="openLink($event)">
        {{ open ? 'expand_less' : 'expand_more' }}
      </i>
      <div class="empty-icon" v-else></div>
      <span>{{title}}</span>
    </div>
    <SidebarLink v-if="links.length && open"
                 v-for="header in links"
                 :item="header"/>
  </router-link>
</template>
<script>
  import SidebarLink from './SidebarLink.vue'

  export default {

    name: 'SidebarLink',

    props: ['item'],
    components: { SidebarLink },

    data() {
      return {
        open: false,
        path: '',
        title: '',
        links: []
      }
    },

    methods: {
      openLink(e) {
        e.preventDefault();
        e.stopPropagation();
        this.open = !this.open;
      },
    },

    mounted() {
      this.links = (this.item.headers || []).map(header => ({
        ...header,
        parentLink: this.item.path || this.item.parentLink
      }));
      this.path = (this.item.path || this.item.parentLink) + '#' + (this.item.slug || '');
      this.open = this.item.open;
      this.title = this.item.title;
    }
  }
</script>

<style lang="stylus">
@import './styles/config.styl'
.empty-icon
  width 16px
  height 16px
.material-icons
  line-height 14px
  width 14px
  height 14px
  color #b5bbc1
.item-wrapper
  padding-left 13px
  cursor pointer
  &.router-link-active
    > .sidebar-link
      border-right 3px solid $accentColor
.sidebar-link
  width 100%
  font-size 14px
  line-height 3.43
  color #314659
  display flex
  align-items center
  > span
    padding-left 21px
  &:hover
    color $accentColor
  .sidebar-sub-headers &
    padding-top 0.25rem
    padding-bottom 0.25rem
    border-left none
    &.active
      font-weight 500
</style>
