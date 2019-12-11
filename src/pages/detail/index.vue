<template>
  <div class="wrap" id="details">
    <div>
      <!-- 头部图片部分 -->
      <div id="head">
        <img :src="detail.image" mode="widthFix" />
      </div>
      <!-- 详情介绍 -->
      <div id="footer">
        <!-- 首部 -->
        <div id="top">
          <p>
            <span id="dangSelef">当当自营</span>
            {{detail.name}}
          </p>
          <p id="desc">{{detail.desc}}</p>
          <p>
            <span id="price">￥{{detail.price}}元</span>
            <span id="count">(9.48折)</span>
            <span id="reduce_price">降价通知</span>
          </p>
          <p>
            定价
            <span id="old_price">￥{{detail.old_price}}元</span>
          </p>
        </div>
        <!-- 尾部 -->
        <div id="bottom">
          <div id="store">
            <img src="/static/images/store.png"  />
            <p>店铺</p>
          </div>
          <div id="detail-cart" @click="toCartPage">
            <img src="/static/images/cart.png"  />
            <p>购物车</p>
          </div>
          <div id="buyBtn">立即购买</div>
          <div id="addCart" @click="addCartGoods(detail.id)">加入购物车</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      bookList: [],
      detail:{}
    };
  },
  onLoad:function(e) {
    let id = e.id;
    let book = this.bookList.find((item)=>{
      return (item.id == id)
    })
    this.detail = book;
  },
  methods: {
    // 添加购物车
    addCartGoods(id){
      // console.log(id)
      // 获取缓存中的cart
      let products = wx.getStorageSync('cart') || [];
      // console.log(products)
      // 根据id判断在购物车（缓存）中是否存在
      let index = products.findIndex((item)=>{
        // console.log(index)
        return item.id == id
      })
      if(index!=-1){
        // id已存在
        products[index].count+=1
      }else{
        // 不存在
        this.detail.count = 1
        this.detail.checked = true 
        // 给cart数组里面追加detail
        products.push(this.detail)
      }
      wx.setStorageSync('cart',products)
      // wx.switchTab({
      //   url: '/pages/cart/main',
      //   success: (res)=>{
      //     // success
      //     console.log(res)
      //   },
      //   fail: (err)=>{
      //     // fail
      //     console.log(err)
      //   }
      // })
    },
    toCartPage(){
      wx.switchTab({
        url: '/pages/cart/main',
        success: function(res){
          // success
          console.log(res)
        },
        fail: function(err) {
          // fail
          console.log(err);
        }
      })
    }
  },
  created() {
    wx.request({
      url: "http://127.0.0.1:8080/book.json",
      success: res => {
        // success
        console.log(res);
        console.log(res.data);
        this.bookList = res.data;
      },
      fail: err => {
        // fail
        console.log(err);
      }
    });
  }
};
</script>
<style scoped>
#desc{
  color: #5f5f5f;
  font-size: 30rpx;
}
#dangSelef{
  color: #ff0000;
  width: 200rpx;
  height: 100%;
  border:1px solid red;
  border-radius: 30rpx;
  padding: 0 10rpx;
}
#price{
  color: red;
}
#old_price{
  text-decoration: line-through;
}
#count{
  color: #bebbbb;
}
#reduce_price {
  width: 200rpx;
  height: 100%;
  border-radius: 30rpx;
  border: 1px solid #5f5f5f;
  padding: 0 20rpx;
  text-align: center;
  color: #5f5f5f;
}
#buyBtn {
  width: 230rpx;
  height: 100%;
  background: #ffbe26;
  text-align: center;
  color: #ffffff;
  line-height: 102rpx;
}
#addCart {
  width: 230rpx;
  height: 100%;
  background: #f3554a;
  text-align: center;
  color: #ffffff;
  line-height: 102rpx;
}
#details {
  width: 750rpx;
  height: 1208rpx;
  display: flex;
  flex-direction: column;
  background: #eeeeee;
  padding: 20rpx;
  box-sizing: border-box;                                                              
}
#head {
  flex: 1;
}
#head img {
  width: 100%;
  height: 100%;
}
#footer {
  width: 100%;
  height: 456rpx;
  background: #f8f8f8;
  display: flex;
  flex-direction: column;
  margin-top: 10rpx;
  margin-bottom: 10rpx;
}
#top {
  flex: 1;
}
#bottom {
  width: 100%;
  height: 102rpx;
  background: ffffff;
  display: flex;
  justify-content: space-between;
}
#store {
  width: 102rpx;
  height: 102rpx;
  background: ffffff;
  text-align: center;
}
#store img {
  width: 40rpx;
  height: 40rpx;
}
#detail-cart img {
  width: 40rpx;
  height: 40rpx;
}
#detail-cart {
  width: 208rpx;
  height: 102rpx;
  background: ffffff;
  text-align: center;
}
</style>