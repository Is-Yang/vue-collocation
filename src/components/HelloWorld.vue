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

    <div class="product-main">
      <yd-cell-group>
        <!-- 步骤条 -->
        <div class="choose-product">
          <yd-cell-item arrow type="label" @click.native="choosePopup=true">
            <span slot="left">选择产品规格</span>
          </yd-cell-item>
          <yd-step :current="currentStep">
            <yd-step-item v-for="item in productItems" :key="item.label">
              <span slot="bottom">{{item.label}}</span>
            </yd-step-item>
          </yd-step>
        </div>

        <!-- 商品详情 -->
        <div class="product-details">
          <yd-cell-item>
            <span slot="left">商品详情</span>
          </yd-cell-item>
          <div class="details-img">
            <img v-for="(img, index) in imgDetails" :key="index" :src="img" />
          </div>
        </div>
      </yd-cell-group>
    </div>

    <yd-popup v-model="choosePopup" position="bottom" height="60%">
      <div class="popup-head">{{productItems[currentIndex].label}}</div>

      <div class="popup-body">

        <div class="product-info">
          <p>{{productItems[currentIndex].title}}</p>
          <span class="price">￥{{currentPrice}}</span>
        </div>

        <div class="product-label">
          <!-- 系列 -->
          <div class="mt-10" v-for="series in productItems[currentIndex].seriesList" :key="series.seriesId">
            <h3>{{series.label}}</h3>
            <div>
              <span class="labelItem" 
                v-for="item in series.item" 
                :key="item.itemId" 
                @click="clickItem(productItems[currentIndex].form, item)"
                :class="{'active': item.itemId == activeLabel}">{{item.label}}</span>
            </div>
          </div>

          <!-- 颜色 -->
          <div class="mt-10">
            <h3>颜色</h3>
            <div>
              <span class="labelItem" 
                v-for="(color, index) in colorList" 
                :key="index"
                @click="clickColor(color)"
                :class="{'active': color == activeColor}">{{color}}</span>
            </div>
          </div>

          <!-- 尺码 -->
          <div class="mt-10">
            <h3>尺码</h3>
            <div>
              <span class="labelItem" 
                v-for="(size, index) in sizeList" 
                :key="index"
                @click="clickSize(size)"
                :class="{'active': size == activeSize}">{{size}}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="popup-foot">
        <yd-button v-if="flag != 'A' && flag != ''" size="large" type="hollow" @click.native="from('prev', flag)">上一步</yd-button>
        <yd-button v-if="flag != 'D'" size="large" :type="isNext ? 'hollow' : 'disabled'" @click.native="from('next', flag)">下一步</yd-button>
        <yd-button v-if="flag == 'C' || flag == 'D'" size="large" type="danger" @click.native="submit()">加入购物车</yd-button>
      </div>
    </yd-popup>

    <a href="javascirpt:;" class="go-top" :style="{display: isDisplay}" @click="goTop()"></a>
  </div>
</template>

<script>
  import {
    GridsGroup,
    GridsItem
  } from 'vue-ydui/dist/lib.rem/grids';
  import {
    CellGroup,
    CellItem
  } from 'vue-ydui/dist/lib.rem/cell';
  import {
    Step,
    StepItem
  } from 'vue-ydui/dist/lib.rem/step';
  import {
    Button,
    ButtonGroup
  } from 'vue-ydui/dist/lib.rem/button';
  import {
    Popup
  } from 'vue-ydui/dist/lib.rem/popup';

  import {
    Toast,
    Loading
  } from 'vue-ydui/dist/lib.rem/dialog';

  export default {
    data() {
      return {
        product: {
          a: "/static/image/a.png",
          b: "/static/image/b.png",
          c: "/static/image/c.png",
          d: "/static/image/d.png"
        },
        productItems: [
          {
            id: 1, // 产品ID
            form: 'A', // 产品标识A/B/C/D
            label: 'A产品', // 产品标签
            title: 'AAA产品描述xxxxx', // 产品标题
            seriesList: [{ // 产品系列
              seriesId: 11, // 产品系列id
              label: 'A-可爱系列', // 产品系列标签
              item: [{ // 产品系列下的信息
                itemId: 111,
                label: 'A1',
                img: '/static/image/test/c1.jpg',
                price: 120,
                colors: ['红色', '蓝色', '紫色', '粉色'],
                sizes: ['21', '22', '34', '43']
              }, {
                itemId: 112,
                label: 'A1',
                img: '/static/image/test/b2.jpg',
                price: 122,
                colors: ['绿色', '蓝色', '紫色', '粉色'],
                sizes: ['21', '22', '34', '43']
              }, {
                itemId: 113,
                label: 'A1',
                img: '/static/image/test/b1.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 114,
                label: 'A1',
                img: '/static/image/test/b4.jpg',
                price: 125,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }, {
              seriesId: 12,
              label: 'A-OL系列',
              item: [{
                itemId: 121,
                label: 'A1',
                img: '/static/image/test/c1.jpg',
                price: 120,
                colors: ['红色', '蓝色', '紫色', '粉色'],
                sizes: ['21', '22', '34', '43']
              }, {
                itemId: 122,
                label: 'A1',
                img: '/static/image/test/c4.jpg',
                price: 121,
                colors: ['绿色', '蓝色', '紫色', '粉色'],
                sizes: ['21', '22', '34', '43']
              }, {
                itemId: 123,
                label: 'A1',
                img: '/static/image/test/a2.jpg',
                price: 122,
                colors: ['红色', '灰色', '紫色', '粉色'],
                sizes: ['21', '62', '34', '44']
              }, {
                itemId: 124,
                label: 'A1',
                img: '/static/image/test/a1.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }, {
              seriesId: 13,
              label: 'A-青春系列',
              item: [{
                itemId: 131,
                label: 'A3',
                img: '/static/image/test/d4.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 132,
                label: 'A3',
                img: '/static/image/test/d3.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 133,
                label: 'A3',
                img: '/static/image/test/d2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 134,
                label: 'A3',
                img: '/static/image/test/d1.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }]
          },
          {
            id: 2,
            form: 'B', // 产品标识A/B/C/D
            label: 'B产品',
            title: 'BBB产品描述xxxxx',
            price: 140,
            seriesList: [{
              seriesId: 21,
              label: 'B-可爱系列',
              item: [{
                itemId: 211,
                label: 'A3',
                img: '/static/image/test/c4.jpg',
                price: 211,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 212,
                label: 'A3',
                img: '/static/image/test/c3.jpg',
                price: 213,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 213,
                label: 'A3',
                img: '/static/image/test/c2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 214,
                label: 'A3',
                img: '/static/image/test/c1.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }, {
              seriesId: 22,
              label: 'B-OL系列',
              item: [{
                itemId: 221,
                label: 'A3',
                img: '/static/image/test/b4.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 222,
                label: 'A3',
                img: '/static/image/test/b1.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 223,
                label: 'A3',
                img: '/static/image/test/a2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 224,
                label: 'A3',
                img: '/static/image/test/a1.jpg',
                price: 224,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }]
          },
          {
            id: 3,
            form: 'C', // 产品标识A/B/C/D
            label: 'C产品',
            title: 'CCC产品描述xxxxx',
            price: 180,
            seriesList: [{
              seriesId: 34,
              label: 'B-可爱系列',
              item: [{
                itemId: 341,
                label: 'A3',
                img: '/static/image/test/b4.jpg',
                price: 333,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 342,
                label: 'A3',
                img: '/static/image/test/b3.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 343,
                label: 'A3',
                img: '/static/image/test/c2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 344,
                label: 'A3',
                img: '/static/image/test/c1.jpg',
                price: 443,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }, {
              seriesId: 36,
              label: 'B-校园系列'
            }]
          },
          {
            id: 4,
            form: 'D', // 产品标识A/B/C/D
            label: 'D产品',
            title: 'DDD产品描述xxxxx',
            price: 200,
            seriesList: [{
              seriesId: 42,
              label: 'D-OL系列',
              item: [{
                itemId: 421,
                label: 'A3',
                img: '/static/image/test/c2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 422,
                label: 'A3',
                img: '/static/image/test/a2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 423,
                label: 'A3',
                img: '/static/image/test/d2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 424,
                label: 'A3',
                img: '/static/image/test/b2.jpg',
                price: 123,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }, {
              seriesId: 43,
              label: 'D-校园系列',
              item: [{
                itemId: 431,
                label: 'A3',
                img: '/static/image/test/c1.jpg',
                price: 1000,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 432,
                label: 'A3',
                img: '/static/image/test/a1.jpg',
                price: 1000,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 433,
                label: 'A3',
                img: '/static/image/test/b1.jpg',
                price: 1000,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }, {
                itemId: 434,
                label: 'A3',
                img: '/static/image/test/d1.jpg',
                price: 342,
                colors: ['粉色', '蓝色', '紫色', '粉色'],
                sizes: ['11', '22', '14', '43']
              }]
            }]
          }
        ],
        imgDetails: [
          'https://imageysc.kuaidiantong.cn/Storage/master/gallery/20170626/6363408724866585997873066.jpg',
          'https://imageysc.kuaidiantong.cn/Storage/master/gallery/20170626/6363408724871272483858974.jpg',
          'https://imageysc.kuaidiantong.cn/Storage/master/gallery/20170626/6363408724879089421601545.jpg'
        ],
        flag: '',
        choosePopup: false,
        isNext: false,
        isDisplay: 'none',
        currentIndex: 0,
        currentPrice: 0,
        currentStep: 0,
        activeLabel: 0,
        activeColor: '',
        activeSize: '',
        colorList: [],
        sizeList: []
      }
    },
    mounted() {
      window.addEventListener('resize', this.handleResize, true);
      window.addEventListener('scroll', this.handleScroll, true);
    },
    created() {
      this.init();

      setTimeout(() => {
        this.handleResize();
      }, 100);
    },
    methods: {
      init() {
        axios.get('/wap/auth_api/get_product_list_comb')
        .then(function (response) {
          console.log(response)
          if (response.status === 200 && data) {
            
          }
        })
        .catch(function (error) {
          Toast({
            mes: error,
            timeout: 1000
          });
        });
      },
      from(come, flag) {
        Loading.open('正在加载');
        if (come == 'prev') {  // 上一步
          this.currentIndex = flag === 'B' ? 0 : flag === 'C' ? 1 : flag === 'D' ? 2 : 3;
        } else {  // 下一步
          this.isNext = false;
          this.currentIndex = flag === 'A' ? 1 : flag === 'B' ? 2 : flag === 'C' ? 3 : 0;
        }
        this.flag = this.currentIndex === 0 ? 'A' : this.currentIndex === 1 ? 'B' : this.currentIndex === 2 ? 'C' : 'D'; 
        this.currentStep = this.currentIndex;
        Loading.close();
      },
      clickItem(form, item) {
        if (form === 'A') {
          this.product.a = item.img;
        } else if (form === 'B') {
          this.product.b = item.img;
        } else if (form === 'C') {
          this.product.c = item.img;
        } else if (form === 'D') {
          this.product.d = item.img;
        }
        this.currentPrice = item.price;
        this.colorList = item.colors;
        this.sizeList = item.sizes;
        this.activeLabel = item.itemId;
        this.flag = form;
        this.isNext = true;
      },
      clickColor(color) {
        this.activeColor = color;
        console.log(color);
      },
      clickSize(size) {
        this.activeSize = size;
        console.log(size);
      },
      // 监听show-product容器宽度
      handleResize() {
        let showProduct = document.getElementsByClassName("show-product")[0];
        let bodyProduct = document.getElementsByClassName("product-main")[0];
        let tabWrapper = document.getElementsByClassName("tab-wrapper")[0];
        let height = document.documentElement.clientHeight || document.body.clientHeight;
        let footerBtn = document.getElementsByClassName("footer-btn")[0];
        if (showProduct) {
          let width = showProduct.clientWidth;
          showProduct.style.height = width + 'px';

          if (bodyProduct) {
            bodyProduct.style.marginTop = width + 10 + 'px';
            bodyProduct.style.height = height - width + 'px';
          }

        }
      },
      handleScroll() {
        let bodyProduct = document.getElementsByClassName("product-main")[0];
        if (bodyProduct) {
          let scrollTop = bodyProduct.pageYOffset || bodyProduct.scrollTop;
          scrollTop > 100 ? this.isDisplay = 'block' : this.isDisplay = 'none';
        }
      },
      goTop() {
        let bodyProduct = document.getElementsByClassName("product-main")[0];
        if (bodyProduct) {
          bodyProduct.scrollTop = 0;
        }
      },
      submit() {
        this.$dialog.toast({
          mes: '提交',
          timeout: 1000
        });
        this.currentStep = 4;
      }
    },
  }

</script>

<style lang="less">
  body {
    width: 100%;
    max-width: 768px;
    margin: 0 auto !important;
    overflow: hidden;
  }

  .product-container {
    .show-product {
      width: 100%;
      max-width: 768px;
      height: 375px;
      position: fixed;
      top: 0;
      left: 50%;
      right: 0;
      transform: translateX(-50%);

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
              position: absolute;
              left: 50%;
              top: 50%;
              -webkit-transform: translate3d(-50%, -50%, 0);
              transform: translate3d(-50%, -50%, 0);
              max-width: 100%;
              max-height: 100%;
              line-height: 100%;
              visibility: middle;
            }
          }
        }
      }
    }

    .product-main {
      margin-top: 385px;
      margin-bottom: 50px;
      height: 290px;
      overflow-y: auto;
      overflow-x: hidden;

      .yd-cell-box {
        margin-bottom: 0;

        .yd-cell {
          background-color: inherit;

          .yd-cell-left {
            font-size: .28rem;
          }
        }
      }

      .choose-product {
        background-color: #fff;
        margin-bottom: 10px;

        .yd-step {
          padding: 20px 0 10px 0;
        }
      }

      .product-details {
        background-color: #fff;

        img {
          width: 100% !important;
          height: auto !important;
        }
      }
    }

    .yd-popup-content {
      height: 100%;

      &>div {
        height: 100%;
        display: flex;
        flex-direction: column;

        .popup-head {
          height: .8rem;
          line-height: .83rem;
          background-color: #f5f5f5;
          border-bottom: 1px solid #eee;
          text-align: center;
        }

        .popup-body {
          flex: 1;
          padding: 10px 12px;
          overflow-y: auto;
        }

        .popup-foot {
          display: flex;

          &>button {
            flex: 1;
            border-radius: 0;
            margin: 0;
            border-right: 1px solid #fff;
            border-bottom: 0;
            font-size: .28rem;
            height: .9rem;

            &:nth-child(1) {
              border-left: none;
              border-right: 0;
            }

            &:last-child {
              border-right: 0;
            }
          }
        }
      }
    }

    .product-info {
      padding-bottom: 10px;
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

    .product-label {
      .labelItem {
        display: inline-block;
        margin: .15rem .1rem .13rem 0;
        padding: .1rem .2rem;
        border: 1px solid #c9c9c9;
        border-radius: 4px;
        text-align: center;
        &.active {
          color: #ef4f4f;
          border: 1px solid #ef4f4f;
        }
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

    .mt-10 {
      margin-top: 10px;
    }

    .mb-10 {
      margin-bottom: 10px;
    }
  }

</style>
