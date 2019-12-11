<template>
  <div class="wrap" id="wrap">
    <div id="nav">
      <div id="logo">
        <img src="/static/images/logo.png" alt="当当网" />
      </div>
      <div id="search">
        <!-- <i class="icon iconfont icon-shenhe"></i> -->
        <div id="sear">
          <img src="/static/images/search.png" alt="搜索" />
        </div>
      </div>
      <div id="category">
        <img src="/static/images/category.png" alt="分类" />
      </div>
    </div>
    <div id="swiper">
      <swiper
        circular="true"
        autoplay="true"
        interval="5000"
        duration="500"
        id="swip"
        indicator-dots="true"
        indicator-color="rgba(255, 255, 255)"
        indicator-active-color="rgba(255, 0, 0)"
      >
        <swiper-item>
          <img src="/static/images/banner1.jpg" />
        </swiper-item>
        <swiper-item>
          <img src="/static/images/banner2.jpg" />
        </swiper-item>
        <swiper-item>
          <img src="/static/images/banner3.jpg" />
        </swiper-item>
        <swiper-item>
          <img src="/static/images/banner4.jpg" />
        </swiper-item>
        <swiper-item>
          <img src="/static/images/banner5.jpg" />
        </swiper-item>
        <swiper-item>
          <img src="/static/images/banner6.jpg" />
        </swiper-item>
      </swiper>
    </div>
    <div id="Category">
      <div id="cate" v-for="(item,index) in typeList" :key="index">
        <img :src="item.image" alt />
      </div>
    </div>
    <div id="intro">
      <img src="/static/images/nav2.jpg" alt />
    </div>
    <div id="info">
      <div id="everyInfo" v-for="(item,index) in bookList" :key="index" @click="toDetails(item.id)">
        <img :src="item.image"  id="infoImg" />
        <p>{{item.name}}</p>
        <p></p>
        <p>
          <span id="price">￥{{item.price}}元</span>
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      typeList: [],
      bookList: []
    };
  },
  onLoad(e) {
    console.log(e);
    let id = e.id;
    let book = this.bookList.find(item => {
      return (item.id == id);
    });
  },
  methods: {
    toDetails(i) {
      wx.navigateTo({
        url: `/pages/detail/main?id=${i}`,
        success: function(res) {
          // success
          // console.log("我跳转了");
          console.log(res);
        },
        fail: function(err) {
          // fail
          console.log(err);
        }
      });
    }
  },
  created() {
    wx.request({
      url: "http://127.0.0.1:8080/type.json",
      success: res => {
        console.log(res.data);
        this.typeList = res.data;
      }
    });
    wx.request({
      url: "http://127.0.0.1:8080/book.json",
      success: res => {
        // console.log(res);
        this.bookList = res.data;
      }
    });
  }
};
</script>
<style scoped>
.icon,
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
#price {
  color: red;
}
#info {
  width: 100%;
  height: 1000rpx;
  background: #ffffff;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin-top: 20rpx;
}
#everyInfo {
  width: 350rpx;
  height: 350rpx;
}
#infoImg {
  width: 350rpx;
  height: 350rpx;
}
#everyInfo a {
  display: inline-block;
}
#intro {
  width: 750rpx;
  height: 202rpx;
}
#intro img {
  width: 750rpx;
  height: 202rpx;
}
#Category {
  width: 100%;
  height: 308rpx;
  background: #ffffff;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-bottom: 10rpx;
}
#cate {
  width: 150rpx;
  height: 150rpx;
}
#cate img {
  width: 150rpx;
  height: 150rpx;
}
#wrap {
  width: 750rpx;
  height: 100%;
  background: #eeeeee;
}
#nav {
  width: 750rpx;
  height: 86rpx;
  background: #ffffff;
  display: flex;
}
#logo {
  width: 86rpx;
  height: 100%;
  margin-right: 30rpx;
}
#logo img {
  width: 86rpx;
  height: 86rpx;
}
#search {
  flex: 1;
}
#sear {
  border: 2px solid #dddddd;
  border-radius: 35rpx;
  width: 500rpx;
  height: 60rpx;
  background: #cfcfcf;
  margin-top: 10rpx;
}
#sear img {
  width: 40rpx;
  height: 40rpx;
  margin-left: 10rpx;
  margin-top: 10rpx;
}
#category {
  width: 86rpx;
  height: 100%;
  margin-left: 30rpx;
}
#category img {
  width: 86rpx;
  height: 86rpx;
}
#swiper {
  width: 100%;
  height: 218rpx;
  background: #ffffff;
}
#swip {
  width: 100%;
  height: 218rpx;
}
#swip swiper-item img {
  width: 100%;
  height: 218rpx;
}
</style>