<!-- 首页 -->
<template>
  <div class="fe-page home-page">

    <div class="fe-scroll-y" style="position: relative">
      <!--      导航栏-->
      <!--      header-->
      <header>
        <Nav></Nav>
        <!--    banner-->
        <div style="width: 100%; min-width: 1180px; display: flex; align-items: center">
          <div class="w-100">
            <div class="bannerShu" ref="banner">
              <transition-group tag="ul" class="slide-ul" name="list">
                <li
                  v-for="(item, index) in bannerImgArray"
                  :key="item.title"
                  v-show="bannerNum === index"
                  @mouseover="stopPlay()"
                  @mouseleave="bannerShu()"
                >
                  <div
                    class="flex-column-center fe-page"
                    @click="jump(item.url)"
                    style="position: absolute"
                  >
                    <div class="bannerText">
                      <div :id="bannerImgArrayId[index].title" class="line1">
                        {{ item.title }}
                      </div>
                      <div :id="bannerImgArrayId[index].describe" class="line2">
                        {{ item.describe }}
                      </div>

                    </div>
                  </div>
                  <img
                    :id="bannerImgArrayId[index].src"
                    width="100%"
                    :src="item.src"
                    alt=""
                  />
                </li>
              </transition-group>
            </div>
          </div>
        </div>
      </header>
      <!--      container-->
      <div class="w-100" style="overflow-y: hidden">
        <!--        热门产品-->
        <div style="height: 785px; background-color: #ffffff">
          <div class="fe-page-container">
            <div class="container-title flex-column-between">
              <p :id="containerElement1Id.title">{{ containerElement1.title }}</p>
              <p :id="containerElement1Id.title" class="title-describe">
                {{ containerElement1.describe }}
              </p>
            </div>
            <div
              v-if="hotProductId.length"
              class="popular-container fe-flex-center w-100"
            >
              <div
                @click="jump(item.url)"
                class="h-100 popular-content"
                v-for="(item, index) in hotProduct"
                @mouseover="hotOver(index)"
                @mouseleave="hotLeave()"
                :key="item.title"
              >
                <div class="popular-title flex-column-between">
                  <img
                    v-if="activeHot === index"
                    :src="item.img[1]"
                    :id="hotProductId[index].img[1]"
                    alt=""
                    width="auto"
                    height="38px"
                  />
                  <img
                    v-if="activeHot !== index"
                    :src="item.img[0]"
                    :id="hotProductId[index].img[0]"
                    alt=""
                    width="auto"
                    height="38px"
                  />
                  <p :id="hotProductId[index].title">{{ item.title }}</p>
                </div>
                <div class="popular-bottom flex-column-between">
                  <ul :id="hotProductId[index].tag" class="flex-column-between">
                    <li v-for="tag in item.tag" :key="tag" class="fe-flex">

                      <span>{{ tag }}</span>
                    </li>
                  </ul>
                  <div style="height: 54px" class="popular-price flex-column-between">
                    <p>
                      价格：
                      <span
                        :id="hotProductId[index]"
                        class="fe-font-lg fe-mr-sm"
                        style="color: #ff956f"
                      >
                        ￥{{ item }}.
                        <span class="fe-font-md">00</span>
                      </span>
                      <span :id="hotProductId[index].unit" style="color: #455a74">{{
                          item.unit
                        }}</span>
                    </p>
                    <p>
                      优惠：<span
                      :id="hotProductId[index]"
                      style="color: #455a74"
                    >{{ item }}</span
                    >
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div id="e12345678" class="w-100 fe-flex-center" style="margin-top: 80px">
              <div @click="jump(moreProducts.url)" class="more-product fe-flex-center">
                {{ moreProducts.title }}
              </div>
            </div>
          </div>
        </div>
        <!--        专网组合产品推荐-->
        <div style="height: 670px; background-color: #f9f9f9">
          <div class="fe-page-container">
            <div class="container-title flex-column-between">
              <p :id="containerElement2Id.title">{{ containerElement2.title }}</p>
              <p :id="containerElement2Id.describe" class="title-describe">
                {{ containerElement2.describe }}
              </p>
            </div>
            <div class="fe-flex-center" style="height: 100px; margin-bottom: 23px">
              <div
                class="recommend-button"
                v-for="(item, index) in recommendation"
                @click="changeSemester(index)"
                @mouseover="recommendationOver(index)"
                @mouseleave="recommendationLeave"
                :class="{ activeBar: index === active }"
                :key="item.title"
              >
                <img
                  :id="recommendationId[index].img[0]"
                  v-if="index === active"
                  :src="item.img[0]"
                  alt=""
                  height="48px"
                />
                <img
                  v-if="index !== active && index === activeHover"
                  :id="recommendationId[index].img[0]"
                  :src="item.img[0]"
                  alt=""
                  height="48px"
                />
                <img
                  v-if="index !== active && index !== activeHover"
                  :id="recommendationId[index].img[1]"
                  :src="item.img[1]"
                  alt=""
                  height="48px"
                />
                <p :id="recommendationId[index].title">{{ item.title }}</p>
              </div>
            </div>
            <div v-if="recommendation[active]" class="product">
              <div class="product-pre">
                <div class="fe-page pre-container fe-flex-column">
                  <p :id="recommendationId[active]" style="font-size: 20px">
                    {{ recommendation[active] }}
                  </p>
                  <p
                    :id="recommendationId[active]"
                    class="fe-flex pre-describe"
                  >
                    {{ recommendation[active] }}
                  </p>
                </div>
              </div>
              <div
                class="product-end"
                v-for="(item, index) in recommendation[active]"
                :key="item.title"
              >
                <div class="fe-page end-container flex-column-between">
                  <div class="end-container-top">
                    <p
                      :id="recommendationId[active][index].title"
                      class="end-container-title"
                    >
                      {{ item.title }}
                    </p>
                    <!--                    <p class='end-container-describe'>{{ item.describe }}</p>-->
                  </div>
                  <ul
                    :id="recommendationId[active][index].tag"
                    class="end-container-tag"
                  >
                    <li class="fe-flex-center" v-for="tag in item.tag" :key="tag">
                      {{ tag }}
                    </li>
                  </ul>
                  <div class="end-container-group fe-flex-between">
                    <div>
                      <span
                        :id="recommendationId[active][index]"
                        style="color: #ff956f"
                      >￥{{ item }}.00 </span
                      ><span :id="recommendationId[active][index].unit">{{
                        item.unit
                      }}</span>
                    </div>

                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!--        精选方案-->
        <div style="height: 868px; background-color: #ffffff">
          <div class="fe-page-container">
            <div class="container-title flex-column-between">
              <p :id="containerElement3Id.title">{{ containerElement3.title }}</p>
              <p :id="containerElement3Id.describe" class="title-describe">
                {{ containerElement3.describe }}
              </p>
            </div>
            <div v-if="schemeMenu.length" class="scheme w-100">
              <ul class="scheme-menu">
                <li
                  v-for="(item, index) in schemeMenu"
                  :key="item.tab"
                  :id="schemeMenuId[index].tab"
                  @click="changeMenu(index)"
                  :class="{ menuActive: index === menuActive }"
                >
                  {{ item.tab }}
                </li>
              </ul>
              <img
                :src="schemeMenu[menuActive].img"
                alt=""
                :id="schemeMenuId[menuActive].img"
                width="100%"
                height="100%"
                style="
                  position: absolute;
                  z-index: 0;
                  transition: opacity 1s linear;
                  cursor: pointer;
                "
                @click="jump(schemeMenu[menuActive].link)"
              />
              <div class="scheme-bottom flex-column-between">
                <p :id="schemeMenuId[menuActive].title">
                  {{ schemeMenu[menuActive].title }}
                </p>
                <p :id="schemeMenuId[menuActive].value" class="bottom-describe">
                  方案价值：
                  <span v-for="(item, index) in schemeMenu[menuActive].value" :key="item">
                    <span class="vertical-bar" v-if="index !== 0"/>
                    {{ item }}
                  </span>
                </p>
              </div>
            </div>
          </div>
        </div>
        <!--        行业资讯-->
        <div style="height: 852px; background-color: #f9f9f9">
          <div class="fe-page-container">
            <div class="container-title flex-column-between">
              <p :id="containerElement4Id.title">{{ containerElement4.title }}</p>
              <p :id="containerElement4Id.describe" class="title-describe">
                {{ containerElement4.describe }}
              </p>
            </div>
            <ul class="information fe-flex-between">
              <li
                v-for="(item, index) in information"
                @click="jump(item.url)"
                :key="item.title"
              >
                <div class="information-img">
                  <img
                    :id="informationId[index].img"
                    :src="item.img"
                    alt=""
                    width="100%"
                    height="100%"
                  />
                </div>
                <div class="information-container flex-column-between">
                  <p :id="informationId[index].title" class="information-container-title">
                    {{ item.title }}
                  </p>
                  <p
                    :id="informationId[index]"
                    class="information-container-describe"
                  >
                    {{ item }}
                  </p>
                  <p :id="informationId[index].time" class="information-container-time">
                    {{ item.time }}
                  </p>
                </div>
              </li>
            </ul>
          </div>
          <div class="w-100 fe-flex-center" style="margin-top: 80px">
            <div @click="jump(moreInformation.url)" class="more-product fe-flex-center">
              {{ moreInformation.title }}<i class="el-icon-back"/>
            </div>
          </div>
        </div>
      </div>
      <!--      footer-->
      <div style="width: 100%">
        <Footer></Footer>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      bannerNum: 0,
      bannerImgArray: [],
      hotProduct: [],
      recommendation: [],
      schemeMenu: [],
      information: [],

      bannerImgArrayId: [],
      hotProductId: [],
      recommendationId: [],
      schemeMenuId: [],
      informationId: [],
      active: 0,
      activeHover: '',
      activeHot: '',
      menuActive: 0,

      containerElement1: {
        title: '热门产品',
        describe: '为您提供专网自服务建设的热门产品。'
      },
      containerElement2: {
        title: '专网组合产品推荐',
        describe: '针对不同的业务需求，为您提供合适的组合产品。'
      },
      containerElement3: {
        title: '精选方案',
        describe: '为您精选包含最专业的、15大行业的5G专网解决方案。'
      },
      containerElement4: {
        title: '行业资讯',
        describe: '带给您最新的深度行业资讯。'
      },
      containerElement1Id: {},
      containerElement2Id: {},
      containerElement3Id: {},
      containerElement4Id: {},
      moreInformation: {
        title: '更多资讯',
        url: ''
      },
      moreProducts: {
        title: '更多产品',
        url: ''
      }
    }
  },
  computed: {
  },
  methods: {
    // 查询终端商品列表信息
    getInfoPage() {

    },
   
    // 跳转
    jump(index) {
      window.location.href = index
    },
    // banner元素
    
    bannerShu() {
      this.timer1 = setInterval(this.autoPlay, 5000)
    },
    autoPlay() {
      this.bannerNum++
      let a = 1
      if (a === 0) {
        this.bannerNum = 0
      }
    },
    stopPlay() {
      clearInterval(this.timer1)
    },
    // banner高度自适应
    autoHeightS() {
      this.$refs.banner.style.height = this.$refs.banner.clientWidth * 0.3125 + 'px'
      // console.log("宽",this.$refs.banner.clientWidth)
    },
    // Although come back to normal life,we still admire those who see the daylight in fearless years.

    changeSemester(index) {
      this.active = index
    },
    changeMenu(index) {
      this.menuActive = index
    },
    recommendationOver(index) {
      this.activeHover = index
    },
    recommendationLeave() {
      this.activeHover = ''
    },
    hotOver(index) {
      this.activeHot = index
    },
    hotLeave() {
      this.activeHot = ''
    },

  },
  mounted() {
    this.$nextTick(function () {
      window.onresize = () => {
        this.autoHeightS()
      }
      this.autoHeightS()
      this.bannerShu()
      this.getInfoPage()
    })
  },
  beforeDestroy() {
    clearInterval(this.timer1)
  }
}
</script>

<style scoped>

</style>
