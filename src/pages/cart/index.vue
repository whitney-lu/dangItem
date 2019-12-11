<template>
  <div class="wrap" id="cart">
    <div id="cartPage">
      <div id="eachOne">
        <div id="one" v-for="(item,index) in products" :key="index">
          <div id="top">
            <div id="left" @click="onChecked(index)">
              <radio color="#ff0000" :checked="item.checked" id="singleCheck"/>
            </div>
            <div id="mid">
              <img :src="item.image" mode="widthFix" />
            </div>
            <div id="right">
              <p id="text">
                <span>{{item.name}}</span>
                <span class="delete" @click="deleteOne(index)">X</span>
              </p>
              <p id="pric"> 
                <span id="newPrice">￥{{item.price}}</span>
                <span id="oldPrice">￥{{item.old_price}}</span>
                <span id="btn">
                  <button id="reduce" @click="reduce(index)">-</button>
                  <span id="count">{{item.count}}</span>
                  <button id="add" @click="add(index)">+</button>
                </span>
              </p>
            </div>
          </div>
          <div id="bottom">
            <span id="addPrice">加购价</span>
            <span id="discount">购买1件，即可享受换购优惠</span>
            <span id="choose">去选择 ></span>
          </div>
        </div>
      </div>
      <div id="account">
        <span id="allChoose"  @click="allChecked">
          <radio :checked="isCheckAll" color="#ff0000" />
        </span>
        <span id="allChecked">全选</span>
        <span id="totalPrice">合计： ￥ {{total}}</span>
        <span id="accountBtn">结算（3）</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      products: [],
      total: 0,
      isCheckAll:true
    };
  },
  onShow() {
    let products = wx.getStorageSync("cart");
    this.products = products;
    this.getTotalPrice();
  },
  methods: {
    // 点击单选
    onChecked(index) {
      // console.log(index)
      // 单选的时候也要重新设置缓存，并计算总价
      // console.log("我被选中了");
      // 先改变checked的值，取反
      this.products[index].checked = !this.products[index].checked;
      // 然后重新设置缓存，改变缓存里面的checked值
      wx.setStorageSync("cart", this.products);
      // // 遍历一遍数组
      this.isCheckAll = this.products.every(item=>{
        return item.checked == true
      })
      // // 调用计算总价的方法
      this.getTotalPrice();
    },
    // 点击全选
    allChecked() {
      // console.log("我是全选被点击了");
      this.isCheckAll = !this.isCheckAll;
      // 遍历循环数组，当前项和全选项是否一致
      this.products.forEach(item=>{
        item.checked = this.isCheckAll
      })
      wx.setStorageSync('cart', this.products);
      // 再次调用计算总价
      this.getTotalPrice()
    },
    reduce(index) {
      this.products[index].count -= 1;
      if (this.products[index].count < 1) {
        this.products[index].count = 1;
      }
      wx.setStorageSync("cart", this.products);
      // 调用计算总价的方法,重新计算总价
      this.getTotalPrice();
    },
    add(index) {
      this.products[index].count += 1;
      wx.setStorageSync("cart", this.products);
      // 调用计算总价的方法,重新计算总价
      this.getTotalPrice();
    },
    // 计算总价
    getTotalPrice() {
      let products = wx.getStorageSync("cart") || [];
      let total = 0;
      this.products.forEach(item => {
        if (item.checked == true) {
          // 每次做一次累加
          total += item.price * item.count;
        }
        this.total = total;
      });
    },
    // 删除一个
    deleteOne(index){
      this.products.splice(index,1);
      wx.setStorageSync('cart', this.products);
      this.getTotalPrice()
    }
  }
};
</script>
<style scoped>
.delete{
  color: #ff0000;
  font-weight: bolder;
  text-align: right;
  position: absolute;
  right: 10rpx;
  top:10rpx;
}
#text {
  flex: 1;
  position: relative;
}
#one {
  width: 100%;
  height: 300rpx;
  background: #ffffff;
  display: flex;
  flex-direction: column;
  border-radius: 30rpx;
  padding: 10rpx;
  box-sizing: border-box;
  margin-bottom: 10rpx;
}
#allCHecked {
  width: 150rpx;
  height: 100%;
  text-align: center;
  line-height: 100rpx;
}
#totalPrice {
  flex: 1;
  padding-left: 20rpx;
}
#accountBtn {
  width: 200rpx;
  height: 50rpx;
  text-align: center;
  background: #ff0000;
  line-height: 50rpx;
  color: #ffffff;
  border-radius: 50rpx;
}
#allChoose {
  text-align: center;
}
#right {
  flex: 1;
  padding-left: 25rpx;
  display: flex;
  flex-direction: column;
}
#newPrice {
  color: #ff0000;
  font-size: 35rpx;
  font-weight: bolder;
}
#oldPrice {
  width: 180rpx;
  color: #878787;
  font-size: 26rpx;
  text-decoration: line-through;
}
#pric {
  height: 50rpx;
  display: flex;
  justify-content: space-between;
}
#btn {
  display: flex;
  justify-content: space-around;
}
#reduce {
  width: 50rpx;
  height: 50rpx;
  border-radius: 50%;
  text-align: center;
  line-height: 50rpx;
}
#add {
  width: 50rpx;
  height: 50rpx;
  border-radius: 50%;
  text-align: center;
  line-height: 50rpx;
}
#mid {
  width: 150rpx;
  height: 100%;
  padding: 0 20rpx;
  box-sizing: border-box;
  line-height: 200rpx;
}
#mid img {
  width: 150rpx;
}
#left radio,#left checkbox {
  width: 50rpx;
  height: 100%;
  line-height: 200rpx;
}
#account {
  margin-top: 10rpx;
  padding: 30rpx 0;
  box-sizing: border-box;
  width: 100%;
  height: 100rpx;
  background: #ffffff;
  display: flex;
  font-weight: bolder;
  justify-content: space-between;
}
#cartPage {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
#discount {
  flex: 1;
  color: #878686;
  font-size: 26rpx;
}
#addPrice {
  font-size: 20rpx;
  color: red;
  width: 60rpx;
  height: 40rpx;
  text-align: center;
  line-height: 40rpx;
  border: 1px solid #ff0000;
  padding: 0 10rpx;
  margin-right: 30rpx;
}
#choose {
  width: 130rpx;
  height: 50rpx;
  text-align: center；;
}
#top {
  width: 100%;
  flex: 1;
  display: flex;
  box-sizing: border-box;
}
#bottom {
  width: 100%;
  height: 50rpx;
  display: flex;
}
#eachOne {
  width: 100%;
  flex: 1;
  border-radius: 35rpx;
  display: flex;
  flex-direction: column;
  padding: 10rpx;
  box-sizing: border-box;
}
#cart {
  width: 750rpx;
  height: 1129rpx;
  padding: 20rpx;
  background: #dedede;
  box-sizing: border-box;
}
</style>