<template>
    <div id="app">
        <OpenPlatformHeader />
        <iframe
           style="border:none"
           :width="searchTableWidth"
           :height="searchTableHeight"
           v-bind:src="reportUrl"
         ></iframe>
    </div>
  </template>
  <script>
  import Vue from 'vue'
  import OpenPlatformHeader from '@/components/page/OpenPlatformHeader'
  export default {
    name: 'EmbedPage',
    components: {
        OpenPlatformHeader,
    },
    methods: {
      widthHeight() {
        this.searchTableHeight = window.innerHeight -70;
        this.searchTableWidth = window.innerWidth ;
      },
    },
    data() {
      return {
        reportUrl: 'http://59.208.164.227:60010/modelFactory',
        searchTableHeight: 0,
        searchTableWidth: 0
      }
    },
     mounted() {
      window.onresize = () => {
        this.widthHeight(); // 自适应高宽度
      };
      this.$nextTick(function () {
        this.widthHeight();
      });
    },
    created() {
      // 从路由里动态获取 url地址   具体地址看libs下util.js里的 backendMenuToRoute  方法 
      this.reportUrl = 'http://59.208.164.227:60010/modelFactory'
    },
    watch: {
      '$route': function () {
        // 监听路由变化
        this.reportUrl =  'http://59.208.164.227:60010/modelFactory'
      }
    }
  }
  </script>