<template>
  <div id="openplatformHeader">
    <div class="log-title">
      <img src="../../../public/logo2.png" alt="" />
      <div class="title">孝感市数据中台</div>
      <div class="menus">
        <el-menu
          :default-active="$route.path"
          router
          text-color="#fff"
          background-color="#1890FF"
          active-text-color="#fff"
          class="el-menu-demo"
          mode="horizontal"
          @select="handleSelect"
        >
          <el-menu-item index="/openPlatform/home">首页</el-menu-item>
          <el-menu-item index="/catalogue/datacatalogue">数据目录</el-menu-item>
          <el-menu-item index="/service/serviceitem">事项主线目录</el-menu-item>
          <el-menu-item index="/system/policyrule">政策法规</el-menu-item>
          <el-menu-item index="/system/datastandard">数据规范</el-menu-item>
          <el-menu-item index="/system/helpdocument">帮助文档</el-menu-item>
          <el-menu-item index="/system/FunctionNavigation" v-if="token">功能导航</el-menu-item>
          <!-- <el-menu-item index="/openPlatform/home">资源市场</el-menu-item> -->
          <!-- <el-menu-item index="/Home">后台管理</el-menu-item> -->
          <!-- <el-menu-item :route="`http://59.208.164.227:60010/logging?userName=${this.onUserName()}&path=modelFactory`">可视化建模</el-menu-item>  -->
          <!-- <el-menu-item v-if="token" index="/share/informationsharing">数据共享</el-menu-item> -->
        </el-menu>
      </div>
    </div>
    
    <div class="user-info">
      <!-- <span v-if="!token" style="cursor: pointer" @click="$router.push('/login')">登录</span> -->
      <span v-if="!token" style="cursor: pointer" @click="$router.push('/escsso')">登录</span>
      <div v-else>
        <a-avatar class="avatar" style="margin-right: -5px" :src="userImg" />
        <span style="color: #fff; padding: 0 12px;" slot="reference">
          {{ nickname()}}
          <el-tooltip class="over-item" :content=departName placement="top">
            <span v-show="departName" style="font-size: 10px; color: #fff;">({{ departName }})</span>
          </el-tooltip>
        </span>
        <!-- </el-popover>  -->
        <el-popover popper-class="popover1" placement="top">
          <!-- <p style="color: black;">组织架构</p>
          <p v-show="!isEmptyObj">您暂未归属部门</p>
          <p class="pointer1" v-for="depars in department" :key="depars.id" :class="{'bor-color':orgCode===depars.orgCode}" @click="handleDepartment(depars)">
            {{ depars.departName }} <span v-show="orgCode==depars.orgCode" style="font-size: 12px">（当前所在部门）</span>
          </p>  -->
          <!-- <p class="pointer1"  :class="{active:orgCode===depars.orgCode}" v-for="depars in department" :key="depars.id"  @click="handleDepartment(depars)">
            {{ depars.departName }} <span v-show="orgCode==depars.orgCode" style="font-size: 12px">（当前所在部门）</span>
          </p>-->
          <!-- <div class="pointer" style="font-size: 16px;" @click="handlePath('1')" >资源市场</div> -->
          <div class="pointer" style="margin-bottom: 10px;font-size: 14px;" @click="handlePath('2')" >后台管理</div>
          <!-- <div class="pointer" style="margin-top: 15px;font-size: 16px;" @click="handlePath('3')" >可视化建模</div>
          <div class="pointer" style="margin-top: 15px;font-size: 16px;" @click="handlePath('4')" >数栖平台</div>
          <div class="pointer" style="margin-top: 15px;font-size: 16px;" @click="handlePath('5')" >数据安全平台</div> -->
          <!-- <div class="pointer" style="margin-top: 13px;" @click="handleRouter">系统维护</div> -->
          <div class="pointer" style="margin-bottom: 10px;font-size: 14px;" @click="departmentSwitch(department)" v-has="'isDepartment:switch'">部门切换</div>
          <div class="pointer" style="font-size: 14px;" @click="handleLogout">退出登录</div>
          <i class="el-icon-arrow-down" slot="reference" style="cursor: pointer"></i>
        </el-popover>
        <!-- <a-icon style="color: #fff" type="down" @click="handleLoginSel" /> -->
        <select-modal ref="selectModal"></select-modal>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { mapActions, mapGetters } from 'vuex'
import { USER_INFO } from '@/store/mutation-types'
import { putAction } from '@/api/manage'
import { SSO_HREF } from '../../cas/ssoHref'
import { Message } from 'element-ui'
import { getDepartList, queryUserMassge, getUserInfoToken } from '@/api/api'
import { Base64 } from 'js-base64'
import axios from 'axios'
import Cookies from 'js-cookie'
import { Modal } from 'ant-design-vue'
import store from '@/store'
import { ACCESS_TOKEN } from "@/store/mutation-types"
import SelectModal from './SelectModal.vue'

export default {
  name: 'OpenPlatformHeader',
  components: {SelectModal},
  data() {
    return {
      systeDisabled: false,
      token: '',
      nicknames: '',
      userImg: require('@/assets/om/user.png'),
      activeIndex: this.$route.path,
      userName: '',
      department: {},
      orgCode: '',
      departName: '',
    }
  },
  activated() {},
  created() {
    this.userName = this.userInfo().username
    this.token = JSON.parse(localStorage.getItem('pro__Access-Token'))
    this.fetchDepartList()
    this.fetchUserMassge()
  },
  mounted() {
    // this.token = JSON.parse(localStorage.getItem('pro__Access-Token'))
  },
  computed: {
    isEmptyObj() {
      for (let item in this.department) {
        return true
      }
      return false
    },
  },
  methods: {
    fetchUserMassge() {
      queryUserMassge().then((res) => {
        if (res.success) {
          // this.orgCode = res.body.orgCode
          this.departName = res.body.departName
        }
      })
    },
    handleDepartment(data) {
      return new Promise((resolve, reject) => {
        let obj = {
          orgCode: data.orgCode,
          username: this.userName,
        }
        putAction('/sys/selectDepart', obj).then((res) => {
          if (res.success) {
            const userInfo = res.result.userInfo
            Vue.ls.set(USER_INFO, userInfo, 7 * 24 * 60 * 60 * 1000)
            this.$store.commit('SET_INFO', userInfo)
            //console.log("---切换组织机构---userInfo-------",store.getters.userInfo.orgCode);
            Message({
              message: '切换成功',
              type: 'success',
              customClass: 'messageIndex',
            })
            location.reload()
            resolve()
          } else {
            this.requestFailed(res)
            this.$store.dispatch('Logout')
            reject()
          }
        })
      })
    },
    fetchDepartList() {
      getDepartList(this.userName).then((res) => {
        if (res.code === 200) {
          this.department = res.result
        } else {
          if(res.header.code === 10000001){
            let token = localStorage.getItem('pro__Access-Token')
            if (token) {
              Modal.error({
                title: '登录已过期',
                content: '很抱歉，登录已过期，请重新登录',
                okText: '重新登录',
                mask: false,
                onOk: () => {
                  store.dispatch('Logout').then(() => {
                    Vue.ls.remove(ACCESS_TOKEN)
                    window.location.href = 'http://xgdsj.xiaogan.gov.cn'
                  })
                }
              })          
            }
          }
        }
      })
    },
    onUserName() {
      let userName = this.userInfo().username
      let resUserName = Base64.encode(userName)
      return resUserName
    },
    onRealName() {
      let realName = this.userInfo().realname
      let resRealName = Base64.encode(realName)
      return resRealName
    },
    handlePath(type){
      this.isActive = type
      switch(type){
        case '1':
        window.location.href = '/openPlatform/home'
        break;
        case '2':
        window.location.href = '/Home'
        break;
        case '3':
        window.open(`http://59.208.164.227:60010/logging?userName=${this.onUserName()}&path=modelFactory`, '_blank')
        break;
        case '4':
        var newWin = window.open(`http://59.208.164.226:60001/api/uac/current/user/login?userName=${this.onUserName()}&nickName=${this.onRealName()}`, '_blank');
        setTimeout(() => {
            newWin.location = 'http://59.208.164.226:60001/ent/index.html#/console/overview';
        }, 10);

        // axios.defaults.withCredentials = true
        // axios.post('/api/uac/current/user/login',{
        //   "username": "yuzhimin@admin", "password": "14b8e9b316bc9c490af9dbce64452dc9"
        // }).then(function (res){
        //   document.cookie = "dtwave_uac=nKR2vkMfu3eLqXG0; path=/; domain=59.208.164.226"
        //   console.log(res);
        // }).catch(err =>{
        //   console.log(err);
        // })
        // window.open(`http://59.208.164.226:60001/ent/index.html#/console/overview`, '_blank')
        break;
        case '5':

        getUserInfoToken().then((res) => {
          if (res.success) {
            window.open(`https://59.208.164.228:60004/sign/jwtLogin/new?token=${res.result}`, '_blank');
          }
        })
        // window.open(`https://59.208.164.228:60004/#/portal`, '_blank')
        break;
      }
    },
    handleRouter() {
      this.$router.push({ path: '/systemmaintenance' })
    },
    handleSystembox() {
      this.systeDisabled = !this.systeDisabled
    },
    ...mapActions(['Logout']),
    ...mapGetters(['nickname', 'avatar', 'userInfo']),
    handleSelect(key, keyPath) {},
    handleLogout() {
      let path = this.$route.path
      const that = this
      this.$confirm({
        title: '提示',
        content: '真的要注销登录吗 ?',
        onOk() {
          return that
            .Logout({})
            .then(() => {
              if (
                path == '/service/serviceitem' ||
                path == '/catalogue/datacatalogue' ||
                path == '/share/informationsharing' ||
                path == '/openPlatform/home' ||
                path == '/catalogue/datacatalogue/details' ||
                path == '/systemmaintenance'
              ) {
                // update-begin author:wangshuai date:20200601 for: 退出登录跳转登录页面
                // that.$router.push({ path: '/login' })
                
                // 中科sso退出登录
                // window.location.href = `http://59.208.164.233/cas/logout?service=${SSO_HREF}`
                
                // 数梦sso退出登录地址
                window.location.href = 'http://xgdsj.xiaogan.gov.cn'
                // update-end author:wangshuai date:20200601 for: 退出登录跳转登录页面
                // window.location.reload()
              } else {
                return false
              }
            })
            .catch((err) => {
              // that.$message.error({
              //   title: '错误',
              //   description: err.message,
              // })
              if (
                path == '/service/serviceitem' ||
                path == '/catalogue/datacatalogue' ||
                path == '/share/informationsharing' ||
                path == '/openPlatform/home' ||
                path == '/catalogue/datacatalogue/details' ||
                path == '/systemmaintenance'
              ) {
                // update-begin author:wangshuai date:20200601 for: 退出登录跳转登录页面
                // that.$router.push({ path: '/login' })
                
                // 中科sso退出登录
                // window.location.href = `http://59.208.164.233/cas/logout?service=${SSO_HREF}`
                
                // 数梦sso退出登录地址
                window.location.href = 'http://xgdsj.xiaogan.gov.cn'
                // update-end author:wangshuai date:20200601 for: 退出登录跳转登录页面
                // window.location.reload()
              } else {
                return false
              }
            })
        },
      })
    },
    departmentSwitch(department) {//部门切换
      let that = this
      this.$refs.selectModal.show(department)
    },
  },
  activated() {
    // this.token = JSON.parse(localStorage.getItem('pro__Access-Token'))
  },
}
</script>
<style>
.el-popper {
  min-width: 0px !important;
}
</style>
<style lang="less" scoped>
.el-popover.popover {
}
&.pointer {
  cursor: pointer;
  color: #909399;
}
&.pointer:hover {
  color: #606266;
}
.pointer1 {
  cursor: pointer;
  color: #909399;
}
.bor-color {
  border: 1px solid #1890ff;
}
&.pointer1:hover {
  color: #606266;
}
#openplatformHeader {
  width: 100%;
  height: 64px;
  color: #fff;
  padding: 0 56px 0 56px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #1890ff;
  .log-title {
    display: flex;
    align-items: center;
    img {
      width: 32px;
      // height: 24px;
    }
    .title {
      padding-left: 20px;
      font-size: 20px;
    }
    .menus {
      padding-left: 100px;
    }
  }
  .user-conten {
    cursor: pointer;
    .user-info {
      position: relative;
      /deep/.pointer {
        background: red !important;
      }
    }
    .systembox {
      width: 100px;
      height: 100px;
      z-index: 9999;
      position: absolute;
      top: 60px;
      right: 130px;
      background: #fff;
    }
  }
  /deep/.el-menu.el-menu--horizontal {
    border-bottom: solid 0px #e6e6e6;
  }
  .department {
    position: absolute;
    top: 50px;
    right: 130px;
    // width: 100px;
    padding: 12px 12px;
    color: #909399;
    background-color: #fff;
    z-index: 9999;
    cursor: pointer;
  }
}
.active {
  color: #1890ff !important;
}
.over-item {
  display: inline-block;
  cursor: pointer;
  // width: 180px;
  height: 15px;
  max-width: 18em;//限制字数
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>