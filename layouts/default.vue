<template>
  <v-app light class="nuxtid-no-main" v-resize="onResize" v-scroll="onScroll">
    <v-fade-transition>
      <label class="connection" v-if="!$store.state.isOnline">
        Ops, you are offline.<br>Please connect the network
      </label>
    </v-fade-transition>
    <v-fade-transition>
      <div class="loader-wrapper" v-if="$store.state.splash">
        <div id="loader"></div>
      </div>
    </v-fade-transition>
    <v-navigation-drawer
      :mini-variant="true"
      :clipped="false"
      v-model="drawer"
      fixed
      app
      disable-route-watcher
      hide-overlay
      class="shadow-max z-max"
      :class="{'nuxtid-no-bg': !$store.state.isMobile, 'white': $store.state.isMobile}"
    >
      <v-list @click.native="toggleDrawer()">
        <v-list-tile
          router
          class="mb-4"
          :to="{name: 'index'}"
          :replace="$route.name !== 'index'"
        >
          <v-list-tile-action class="pa-2">
            <img :src="require('~/assets/img/logo-nuxtid.png')" class="max-width r shadow-max" alt="NuxtJs.id - Komunitas NuxtJs Indonesia">
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>NuxtJs.id</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile
          router
          class="my-2"
          :to="{name: item.to}"
          :key="i"
          v-for="(item, i) in items"
          :replace="$route.name !== 'index'"
          exact
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed app :clipped-left="true" class="elevation-0 bg-white" :class="{'elevation-3': $store.state.scrollTop > 32 && $store.state.isMobile}">
      <v-toolbar-side-icon @click="drawer = !drawer" :class="{'ml-100': drawer === true }"></v-toolbar-side-icon>
      <v-spacer></v-spacer>
      <v-toolbar-title><nuxt-link :to="{name: 'index'}" class="green--text" :replace="$route.name !== 'index'">{{ title }}</nuxt-link></v-toolbar-title>
    </v-toolbar>
    <v-content>
      <v-container fluid fill-height class="full-bg-cover" :style="{'background-image': 'url(' + require('~/assets/img/indonesia-map.png') + ')'}">
        <nuxt keep-alive/>
      </v-container>
    </v-content>
    <!-- <v-footer app>
      <v-spacer></v-spacer>
      <span>NuxtJs Indonesia &copy; 2018</span>
    </v-footer> -->
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        drawer: false,
        title: 'nuxtjs.id',
        items: [
          { icon: 'school', title: 'Kelas NuxtJs', to: 'class' },
          { icon: 'people_outline', title: 'Account', to: 'accounts' },
          { icon: 'card_travel', title: 'Jobs', to: 'jobs' },
          { icon: 'info_outline', title: 'Info', to: 'info' }
        ]
      }
    },
    created () {
      console.log('')
      console.log('---------------------------------------------------------------------------------------------------------------')
      console.log('%c:: Love Javascript ? Let\'s join us at "https://github.com/nuxtjs-id" ::', 'color: red; font-size:14px;font-weight:bold;')
      console.log('---------------------------------------------------------------------------------------------------------------')
      console.log('')
      // if (process.env.NODE_ENV !== 'development') {
      //   navigator.onLine ? _self.$store.state.isOnline = true : _self.$store.state.isOnline = false
      // }
      // window.addEventListener('online', _self.networkChange)
      // window.addEventListener('offline', _self.networkChange)
    },
    mounted () {
      let _self = this
      // _self.$store.dispatch('Req', { act: 'getMenus' })
      _self.onResize()
      _self.$store.state.isMobile ? _self.drawer = false : _self.drawer = true
      _self.$store.commit('setSplash', false)
    },
    methods: {
      networkChange (e) {
        let _self = this
        if (e.type === 'offline') {
          _self.$store.state.isOnline = false
        } else {
          _self.$store.state.isOnline = true
        }
      },
      onResize () {
        let _self = this
        _self.$store.state.isMobile = window.innerWidth < 769
        if (_self.$store.state.isMobile) {
          _self.$store.state.loading.top = '28px'
          _self.$store.state.loading.left = '55px'
        } else {
          _self.$store.state.loading.top = '32px'
          _self.$store.state.loading.left = '135px'
        }
      },
      onScroll (e) {
        let _self = this
        _self.$store.state.scrollTop = window.pageYOffset || document.documentElement.scrollTop
      },
      toggleDrawer () {
        let _self = this
        if (_self.$store.state.isMobile) {
          _self.drawer = false
        }
      }
    },
    watch: {
      '$route' (to, from) {
        let _self = this
        if (_self.drawer && _self.$store.state.isMobile) {
          _self.$store.state.loading.left = '137px'
        } else if (!_self.drawer && _self.$store.state.isMobile) {
          _self.$store.state.loading.left = '55px'
        } else if (_self.drawer && !_self.$store.state.isMobile) {
          _self.$store.state.loading.left = '135px'
        } else if (!_self.drawer && !_self.$store.state.isMobile) {
          _self.$store.state.loading.left = '43px'
        }
      }
    }
  }
</script>
