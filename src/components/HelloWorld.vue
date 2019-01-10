<template>
  <div class="product-container">
    <!-- 产品图展示 -->
    <div class="show-product">
      <yd-grids-group :rows="2">
        <yd-grids-item>
          <img slot="else" :src="product.a">
        </yd-grids-item>
        <yd-grids-item>
          <img slot="else" :src="product.b" />
        </yd-grids-item>
      </yd-grids-group>

      <yd-grids-group :rows="2">
        <yd-grids-item>
          <img slot="else" :src="product.c" />
        </yd-grids-item>
        <yd-grids-item>
          <img slot="else" :src="product.d" />
        </yd-grids-item>
      </yd-grids-group>
    </div>

    <yd-tab class="product-body" :callback="fn" :prevent-default="false" :item-click="itemClick">
      <yd-tab-panel v-for="(prodcut, index) in productItems" :label="prodcut.label" :key="prodcut.id" :tabkey="index">
        <div class="product-info">
          <p>{{prodcut.title}}</p>
          <span class="price">￥{{prodcut.price}}</span>
        </div>

        <div class="product-series">
          <div v-for="series in prodcut.seriesList" :label="series.label" :key="series.seriesId" style="margin-bottom: 15px;">
            <h3>{{series.label}}</h3>
            <div>
              <span class="seriesItem" v-for="item in series.item" @click="clickItem(prodcut.form, item.img)" :key="item.itemId">{{item.label}}</span>
            </div>
          </div>
          
          <!-- <yd-tab>
              <yd-tab-panel v-for="series in prodcut.seriesList" :label="series.label" :key="series.seriesId">
                <span class="seriesItem" v-for="item in series.item" @click="clickItem(prodcut.form, item.img)" :key="item.itemId">{{item.label}}</span>
              </yd-tab-panel>
          </yd-tab> -->
        </div>
      </yd-tab-panel>
    </yd-tab>
    <div class="footer-btn" v-if="isOk">
      <yd-button size="large" type="danger" @click.native="submit()">加入购物车</yd-button>
    </div>

    <a href="javascirpt:;" class="go-top" :style="{display: isDisplay}" @click="goTop()"></a>
  </div>
</template>

<script>
  import {
    GridsGroup,
    GridsItem
  } from 'vue-ydui/dist/lib.rem/grids';
  import {
    Tab,
    TabPanel
  } from 'vue-ydui/dist/lib.rem/tab';
  import {
    Button,
    ButtonGroup
  } from 'vue-ydui/dist/lib.rem/button';
  import {
    Spinner
  } from 'vue-ydui/dist/lib.rem/spinner';

  import { Toast } from 'vue-ydui/dist/lib.rem/dialog';

  export default {
    data() {
      return {
        product: {
          a: "/static/image/a.png",
          b: "/static/image/b.png",
          c: "/static/image/c.png",
          d: "/static/image/d.png"
        },
        productItems: [{
            id: 1,  // 产品ID
            form: 'A',  // 产品标识A/B/C/D
            label: 'A产品',  // 产品标签
            title: 'AAA产品描述xxxxx',  // 产品标题
            price: 120, // 产品价格
            seriesList: [{ // 产品系列
              seriesId: 11,// 产品系列id
              label: 'A-可爱系列', // 产品系列标签
              item: [{  // 产品系列下的信息
                itemId: 111,
                label: 'A1',
                img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i2/196993935/O1CN01k0SOrv1ewH0HGaQUT_!!0-item_pic.jpg_250x250.jpg_.webp'
              },{
                itemId: 112,
                label: 'A1',
                img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i4/3477411085/TB2vYUDaPDpK1RjSZFrXXa78VXa_!!3477411085-0-item_pic.jpg_250x250.jpg_.webp'
              },{
                itemId: 113,
                label: 'A1',
                img: 'https://g-search1.alicdn.com/img/bao/uploaded/i4/i4/673750156/O1CN011D1UFQPuFsKDnFH_!!673750156-0-item_pic.jpg_250x250.jpg_.webp'
              },{
                itemId: 114,
                label: 'A1',
                img: 'https://g-search3.alicdn.com/img/bao/uploaded/i4/i1/2576722561/O1CN011Umync55ulgyk2V_!!0-item_pic.jpg_250x250.jpg_.webp'
              }]
            },{
              seriesId: 12,
              label: 'A-OL系列', 
              item: [{
                itemId: 121,
                label: 'A1'
              },{
                itemId: 122,
                label: 'A1'
              },{
                itemId: 123,
                label: 'A1'
              },{
                itemId: 124,
                label: 'A1'
              }]
            },{
              seriesId: 13,
              label: 'A-青春系列',
              item: [{
                itemId: 131,
                label: 'A3'
              },{
                itemId: 132,
                label: 'A3'
              },{
                itemId: 133,
                label: 'A3'
              },{
                itemId: 134,
                label: 'A3'
              }]
            }]
          },
          {
            id: 2,
            form: 'B',  // 产品标识A/B/C/D
            label: 'B产品',
            title: 'BBB产品描述xxxxx',
            price: 140,
            seriesList: [{
              seriesId: 21,
              label: 'B-可爱系列'
            },{
              seriesId: 22,
              label: 'B-OL系列'
            },{
              seriesId: 23,
              label: 'B-校园系列'
            }]
          },
          {
            id: 3,
            form: 'C',  // 产品标识A/B/C/D
            label: 'C产品',
            title: 'CCC产品描述xxxxx',
            price: 180,
            seriesList: [{
              seriesId: 34,
              label: 'B-可爱系列'
            },{
              seriesId: 35,
              label: 'B-OL系列'
            },{
              seriesId: 36,
              label: 'B-校园系列'
            }]
          },
          {
            id: 4,
            form: 'D',  // 产品标识A/B/C/D
            label: 'D产品',
            title: 'DDD产品描述xxxxx',
            price: 200,
            seriesList: [{
              seriesId: 41,
              label: 'D-可爱系列'
            },{
              seriesId: 42,
              label: 'D-OL系列'
            },{
              seriesId: 43,
              label: 'D-校园系列'
            }]
          }
        ],
        form: {
          productNum: 0
        },
        flag: '',
        isOk: false,
        isDisplay: 'none'
      }
    },
    mounted() {
      window.addEventListener('resize', this.handleResize, true);
      window.addEventListener('scroll', this.handleScroll, true);
    },
    created() {
      setTimeout(() => {
        this.handleResize();
      }, 1000);
    },
    methods: {
      clickItem(form, img) {
        if (form === 'A') {
          this.product.a = img;
        } else if (form === 'B') {
          this.product.b = img;
        } else if (form === 'C') {
          this.product.c = img;
        } else if (form === 'D') {
          this.product.d = img;
        }
        this.flag = form;
      },
      fn(label, key) {
        if (key === 2 || key === 3) {
          this.isOk = true;
        } else {
          this.isOk = false;
        }
      },
      itemClick(key) {
      
      },
      // 监听show-product容器宽度
      handleResize() {
        let showProduct = document.getElementsByClassName("show-product")[0];
        let bodyProduct = document.getElementsByClassName("product-body")[0];
        if (showProduct) {
          let width = showProduct.clientWidth + 'px';
          showProduct.style.height = width;
          // bodyProduct.style.marginTop = width;

        }
      },
      handleScroll() {
        let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        scrollTop > 100 ? this.isDisplay = 'block' : this.isDisplay = 'none';
      },
      goTop () {
        document.documentElement.scrollTop = 0;
      },
      submit() {
        this.$dialog.toast({mes: '请选择A，B，C产品', timeout: 1000});
      }
    },
  }

</script>

<style lang="less">
  .product-container {
    .show-product {
      // position: fixed;
      // top: 0;
      // left: 0;
      // right: 0;
      height: 375px;

      &>div {
        height: 50%;

        .yd-grids-2 {
          height: 100%;

          &::before {
            border-bottom: 1px solid #d9d9d9;
          }

          .yd-grids-item {
            height: 100%;
            text-align: center;
            padding: 0;

            img {
              width: 100%;
            }
          }
        }
      }
    }

    .product-body {
      margin-top: 10px;
      margin-bottom: 50px;

      .product-info {
        padding: 10px;
        border-bottom: 1px solid #eee;
        p {
          margin-bottom: 5px;
          line-height: 18px;
          word-break: break-word;
          font-size: 16px;
        }

        .price {
          color: #e4393c;
        }
      }
    }

    .product-series {
      padding: 15px 10px;

      .seriesItem {
        display: inline-block;
        margin: 10px 12px 15px 0;
        padding: 5px 10px;
        border: 1px solid #c9c9c9;
        border-radius: 4px;
        text-align: center;
      }

    }

    .go-top {
      width: 42px;
      height: 42px;
      background: url('/static/image/icon_gotop.png');
      background-size: 100%;
      display: inline-block;
      position: fixed;
      right: 20px;
      bottom: 50px;
      z-index: 99;
    }

    .footer-btn {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;

      &>button {
        flex: 1;
        border-radius: 0;
      }
    }
  }

</style>
