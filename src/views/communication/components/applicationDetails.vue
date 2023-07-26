<template>
  <div id="applicationDetails">
    <div class="detailstitle">
      <div>
        《不动产申请表》下架申请
      </div>
      <i class="el-icon-close" @click="closeDetails"></i>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">权益说明</div>
        <div>2023/05/06 11:23</div>
      </div>
      <div class="text">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. Proin gravida dolor
        sit amet lacus accumsan et viverra justo commodo. Proin sodales pulvinar sic tempor. Sociis natoque penatibus et
        magnis dis parturient montes, nascetur ridiculus mus. Nam fermentum, nulla luctus pharetra vulputate, felis
        tellus mollis orci, sed rhoncus pronin sapien nunc accuan eget.
      </div>
      <div class="download" @click="xiazaidownload">
        <i class="el-icon-download"></i>
        <span>不动产申请表下架申请表.doc</span>
      </div>
    </div>
    <div class="detailsList">
      <div class="detailshrader">
        <div class="title">权益说明</div>
        <div>2023/05/06 11:23</div>
      </div>
      <div class="text">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. Proin gravida dolor
        sit amet lacus accumsan et viverra justo commodo. Proin sodales pulvinar sic tempor. Sociis natoque penatibus et
        magnis dis parturient montes, nascetur ridiculus mus. Nam fermentum, nulla luctus pharetra vulputate, felis
        tellus mollis orci, sed rhoncus pronin sapien nunc accuan eget.
      </div>
      <!-- <div class="download">
        <i class="el-icon-download"></i>
        <span>不动产申请表下架申请表.doc</span>
      </div> -->
    </div>
  </div>
</template>
<script>
export default {
  name: 'applicationDetails',
  data() {
    return {}
  },
  methods: {
    xiazaidownload() {},
    closeDetails() {
      this.$emit('closeDetails', false)
    }
  }
}
</script>

<style lang="scss" scoped>
#applicationDetails {
  width: 100%;
  background-color: #fafafc;
  .detailstitle {
    font-size: 17px;
    color: #191c20;
    padding: 20px 0;
    margin: 0 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .detailsList {
    padding: 15px 0 60px 0;
    margin: 0 35px;
    border-top: 1px solid #edf1f6;
    .detailshrader {
      color: #191c20;
      margin: 15px 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      .title {
        font-size: 17px;
      }
    }
    .text {
      color: #191c20;
      margin-bottom: 20px;
    }
    .download {
      color: #4888ff;
      cursor: pointer;
      //   margin-bottom: 40px;
    }
  }
}
</style>
