<template>
  <div>
    <div class="container">
      <div class="show-product">
        <yd-flexbox>
          <yd-flexbox-item style="padding: 10px;"><img :src="img.a" /></yd-flexbox-item>
          <yd-flexbox-item style="padding: 10px;"><img :src="img.b" /></yd-flexbox-item>
        </yd-flexbox>
        <yd-flexbox>
          <yd-flexbox-item style="padding: 10px;"><img :src="img.c" /></yd-flexbox-item>
          <yd-flexbox-item style="padding: 10px;"><img :src="img.d" /></yd-flexbox-item>
        </yd-flexbox>
      </div>  
      <yd-tab class="product-wrapper">
        <div style="height: 295px; overflow-y: auto;">
          <yd-tab-panel label="A产品">
            <div class="product-info">
              <p>AAA产品描述xxxxx</p>
            </div>

            <div class="product-list">
              <div>
                <h3>A-1</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'a')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
              <div>
                <h3>A-2</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'a')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
              <div>
                <h3>A-3</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'a')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
            </div>
          </yd-tab-panel>
          <yd-tab-panel label="B产品">
            <div class="product-info">
              <p>BBB产品描述xxxxx</p>
            </div>
            <div class="product-list">
              <div>
                <h3>B-1</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'b')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
              <div>
                <h3>B-2</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'b')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
              <div>
                <h3>B-3</h3>
                <div>
                  <img slot="img" @click="clickImg(item.img, 'b')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
              </div>
            </div>
          </yd-tab-panel>
          <yd-tab-panel label="C产品">
            <div class="product-info">
              <p>CCCCC产品描述xxxxx</p>
            </div>
            <div class="product-list">
            <div>
              <h3>C-1</h3>
              <div>
                  <img slot="img" @click="clickImg(item.img, 'c')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
            </div>
            <div>
              <h3>C-2</h3>
              <div>
                  <img slot="img" @click="clickImg(item.img, 'c')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
            </div>
            <div>
              <h3>C-3</h3>
              <div>
                  <img slot="img" @click="clickImg(item.img, 'c')" v-for="(item, key) in list" :key="key" :src="item.img">
                </div>
            </div>
            </div>
          </yd-tab-panel>
          <yd-tab-panel label="D产品">
            <div class="product-info">
              <p>DDDD产品描述xxxxx</p>
            </div>
            <div class="product-list">
            <div>
              <h3>D-1</h3>
              <div>
                <img slot="img" @click="clickImg(item.img, 'd')" v-for="(item, key) in list" :key="key" :src="item.img">
              </div>
            </div>
            <div>
              <h3>D-2</h3>
              <div>
                <img slot="img" @click="clickImg(item.img, 'd')" v-for="(item, key) in list" :key="key" :src="item.img">
              </div>
            </div>
            <div>
              <h3>D-3</h3>
              <div>
                <img slot="img" @click="clickImg(item.img, 'd')" v-for="(item, key) in list" :key="key" :src="item.img">
              </div>
            </div>
            </div>
          </yd-tab-panel>
        </div>
      </yd-tab>
      <div class="footer-btn">
        <!-- <yd-button v-if="isDisabled" size="large" type="disabled" disabled>下一步</yd-button>
        <yd-button v-if="!isDisabled" size="large" type="danger">下一步</yd-button> -->
        <yd-button size="large" type="danger">加入购物车</yd-button>
      </div>
    </div>
  </div>
</template>

<script>
import {Tab, TabPanel} from 'vue-ydui/dist/lib.rem/tab';
export default {
  data () {
    return {
     img: {
       a: "https://img.alicdn.com/i2/2/TB1ohGGdeuSBuNjSsziXXbq8pXa?abtest=&pos=2&abbucket=&acm=03014.1003.1.765824&scm=1007.13143.56636.100200300000000_90x90q90.jpg",
       b: "https://img.alicdn.com/i2/2/TB1hTcrk63z9KJjy0FmXXXiwXXa?abtest=&pos=1&abbucket=&acm=03014.1003.1.765824&scm=1007.13143.56636.100200300000000_90x90q90.jpg",
       c: "https://img.alicdn.com/i2/2/TB1t.e1m2DH8KJjy1XcXXcpdXXa?abtest=&pos=9&abbucket=&acm=03014.1003.1.765824&scm=1007.13143.56636.100200300000000_90x90q90.jpg",
       d: "https://img.alicdn.com/i2/2/TB1dDNYlkfb_uJjSsrbXXb6bVXa?abtest=&pos=7&abbucket=&acm=03014.1003.1.765824&scm=1007.13143.56636.100200300000000_90x90q90.jpg"
     },
     list: [
       {img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i2/196993935/O1CN01k0SOrv1ewH0HGaQUT_!!0-item_pic.jpg_250x250.jpg_.webp'},
       {img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i4/3477411085/TB2vYUDaPDpK1RjSZFrXXa78VXa_!!3477411085-0-item_pic.jpg_250x250.jpg_.webp'},
       {img: 'https://g-search1.alicdn.com/img/bao/uploaded/i4/i4/673750156/O1CN011D1UFQPuFsKDnFH_!!673750156-0-item_pic.jpg_250x250.jpg_.webp' },
       {img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i1/2576722561/O1CN011Umync55ulgyk2V_!!0-item_pic.jpg_250x250.jpg_.webp'},
     ]
    }
  },
  methods: {
    clickImg (img, from) {
      if (from === 'a') {
        this.img.a = img;
      } else if (from === 'b') {
        this.img.b = img;
      } else if (from === 'c') {
        this.img.c = img;
      } else if (from === 'd') {
        this.img.d = img;
      } 
    }
  },
}
</script>

<style lang="less" scoped>
 img {
    width: 100%;
    height: auto;
  }

  .show-product {
    position: fixed;
    top:0;
    height: 280px;
    &>div{
      height: 140px;
      overflow: hidden;
    }
  }

  .product-wrapper {
    margin-top: 280px;
    margin-bottom: 50px;
  }

  .product-info {
    padding: 10px;
    font-size: 15px;
    border-bottom: 1px solid #eee;
  }

  .product-list {
    padding: 10px;

    img {
      width: 60px;
      margin: 15px 15px 15px 0;
      padding: 2px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
   
  }

  .footer-btn {
    position: fixed;
    bottom: 0;
    left:0;
    right: 0;
  }
</style>

