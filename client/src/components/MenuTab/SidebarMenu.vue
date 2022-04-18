<template>
  <sidebar-menu
    class="sidebar"
    :collapsed="collapsed"
    :menu="menu"
    @item-click="onItemClick"
    @collapse="onCollapse"
    />
</template>

<script>
export default {
  data () {
    return {
      collapsed: true,
      menu: [
        {
          header: true,
          title: 'ddongle',
          hiddenOnCollapse: true
        },
        {
          id: 'home',
          title: 'home',
          icon: 'fa fa-home'
        },
        {
          id: 'dash',
          title: 'Dashboard',
          icon: 'fa fa-list'
        }
        // {
        //   title: 'Valheim',
        //   icon: 'fa fa-v',
        //   child: [
        //     {
        //       id: 'v_info',
        //       title: '시세표'
        //     },
        //     {
        //       id: 'v_trade',
        //       title: '거래'
        //     }
        //   ]
        // }
      ]
    }
  },
  methods: {
    onItemClick (e, i) {
      this.$emit('sideBarEvent', {
        type: 'Click',
        id: i.id
      })
    },
    onCollapse (c) {
      console.log('onCollapse')
      this.collapsed = c
    }
  }
}
</script>

<style>
.sidebar.v-sidebar-menu .vsm--toggle-btn:after {
  content: '\f105';
  font-family: "FontAwesome";
  display: inline-block;
}
.sidebar.v-sidebar-menu.vsm_expanded .vsm--toggle-btn:after {
  transform: rotate(180deg);
}
</style>
