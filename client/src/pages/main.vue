<template>
  <div id="mainview">
    <!-- 메인 -->
    <div v-show="visibility.main">
      <h1>{{titleText}}</h1>
      <!-- <img height="500" width="500" v-bind:src="getMainLogoUrl()"/> -->
    </div>
    <!-- 사이드 바 -->
    <SidebarMenu v-on:sideBarEvent="sideBarEvent"/>
    <!-- 시세표 -->
    <!-- <div v-show="visibility.quote">
      <Quote />
    </div> -->
  </div>
</template>

<script>
import SidebarMenu from '../components/MenuTab/SidebarMenu.vue'
// import Quote from '../components//TableTab/Quotation.vue'
// main page를 정의합니다.
export default {
  components: {
    SidebarMenu
  },
  created () {
    // 페이지 최초 생성 시
  },
  mounted () {
    // 페이지 드로잉 이후
    this.getDashboardInfo()
  },
  // binding data
  data () {
    return {
      titleText: 'main',
      dashboardData: null,
      visibility: {
        main: true,
        quote: false
      }
    }
  },
  methods: {
    // 메인 화면의 로고 경로 가져오기
    getMainLogoUrl () {
      var filename = 'cat_punch.png'
      var images = require.context('../assets/images/', false, /\.png$/)
      return images('./' + filename)
    },
    getDashboardInfo () {
      this.$http.post('/api/init/getDashboardData', {
        user: 'admin'
      })
        .then((res) => {
          if (res.data.none || res.data.error) {
            alert('none or error data')
            return
          }
          var data = res.data
          this.dashboardData = data
        })
    },
    sideBarEvent (param) {
      if (param.type === 'Click') {
        console.log(param.id)
        if (param.id === 'v_info') {
          this.visibility.quote = true
          this.visibility.main = false
        }
      }
    }
  }
}
</script>

<style>
#mainview {
  padding-left:50px;
}
</style>
