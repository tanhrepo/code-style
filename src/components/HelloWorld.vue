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
            <div class="bannerShuff" ref="banner">
              <transition-group tag="ul" class="slide-ul" name="list">
                <li
                  v-for="(item, index) in bannerImgArray"
                  :key="item.title"
                  v-show="bannerNum === index"
                  @mouseover="stopPlay()"
                  @mouseleave="bannerShuff()"
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

              <ul class="bannerIndex">
                <li
                  v-for="(item, index) in bannerImgArray"
                  :key="index"
                  @click="bannerIndex(index)"
                  :class="index === bannerNum ? 'bannerIndexOn' : 'bannerIndexLi'"
                ></li>
              </ul>
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
                <img
                  v-if="item.hot === '1'"
                  src="@/assets/images/home/hot.svg"
                  alt=""
                  :id="hotProductId[index].hot"
                  width="79"
                  height="81"
                  style="position: absolute; top: 0; left: 0"
                />
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
                      <img
                        src="@/assets/images/home/ok.svg"
                        alt=""
                        width="16"
                        height="16"
                        class="fe-mr"
                      />
                      <span>{{ tag }}</span>
                    </li>
                  </ul>
                  <div style="height: 54px" class="popular-price flex-column-between">
                    <p>
                      价格：
                      <span
                        :id="hotProductId[index].price"
                        class="fe-font-lg fe-mr-sm"
                        style="color: #ff956f"
                      >
                        ￥{{ item.price }}.
                        <span class="fe-font-md">00</span>
                      </span>
                      <span :id="hotProductId[index].unit" style="color: #455a74">{{
                          item.unit
                        }}</span>
                    </p>
                    <p>
                      优惠：<span
                      :id="hotProductId[index].discounts"
                      style="color: #455a74"
                    >{{ item.discounts }}</span
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
                  <p :id="recommendationId[active].preTitle" style="font-size: 20px">
                    {{ recommendation[active].preTitle }}
                  </p>
                  <p
                    :id="recommendationId[active].preDescribe"
                    class="fe-flex pre-describe"
                  >
                    {{ recommendation[active].preDescribe }}
                  </p>
                </div>
              </div>
              <div
                class="product-end"
                v-for="(item, index) in recommendation[active].commodity"
                :key="item.title"
              >
                <div class="fe-page end-container flex-column-between">
                  <div class="end-container-top">
                    <p
                      :id="recommendationId[active].commodity[index].title"
                      class="end-container-title"
                    >
                      {{ item.title }}
                    </p>
                    <!--                    <p class='end-container-describe'>{{ item.describe }}</p>-->
                  </div>
                  <ul
                    :id="recommendationId[active].commodity[index].tag"
                    class="end-container-tag"
                  >
                    <li class="fe-flex-center" v-for="tag in item.tag" :key="tag">
                      {{ tag }}
                    </li>
                  </ul>
                  <div class="end-container-group fe-flex-between">
                    <div>
                      <span
                        :id="recommendationId[active].commodity[index].price"
                        style="color: #ff956f"
                      >￥{{ item.price }}.00 </span
                      ><span :id="recommendationId[active].commodity[index].unit">{{
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
                    :id="informationId[index].container"
                    class="information-container-describe"
                  >
                    {{ item.container }}
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
import Footer from '@/views/components/Footer'
import NavbarHome from '@/views/components/NavbarHome'
// import OperationGuidance from '../components/OperationGuidance.vue'
import Services from '@/api/services'
import {getAction} from '@/api/manage'
import {frontEight, splitStr, parentCode} from '@/utils/mapInfo'
import {mapState, mapMutations} from 'vuex'

const getInfoByPage = Services.mall.getInfoByPage
export default {
  components: {NavbarHome, Footer},
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
    ...mapState('smartGuide', ['smartGuideInfo'])
  },
  methods: {
    ...mapMutations('smartGuide', ['setSmartGuideInfo']),
    // 查询终端商品列表信息
    getInfoPage() {
      let params = {
        pageId: '0001'
      }
      getAction(this.$config.API.CM, getInfoByPage, params).then(res => {
        if (res.success) {
          // let parent = parentCode(res.data,"000100130001")
          // this.data = grouping(res.data)
          res.data.reverse()
          this.bannerImgElement(res.data)
          this.hotProductElement(res.data)
          this.recommendationElement(res.data)
          this.selectionSchemeElement(res.data)
          this.informationElement(res.data)
          this.containerTitleElement(res.data)
          let eId = this.$route.query.eId
          // this.goAnchor("e12345678")
          if (eId) {
            this.$nextTick(() => {
              this.goAnchor('e' + eId)
            })
          }
        } else {
          if (res.fail) {
            this.$message.warning(res.fail.message)
          } else {
            this.$message.warning('查询元素接口异常')
          }
        }
      })
    },
    goAnchor(selector) {
      document.querySelector('#' + selector).scrollIntoView(true)
    },
    // 跳转
    jump(index) {
      // this.$router.push({ name: index })
      // console.log('index', index)
      window.location.href = index
    },
    // banner元素
    bannerImgElement(data) {
      let bannerImgTitle = frontEight(data, '00010001')
      let bannerImgDescribe = frontEight(data, '00010002')
      let bannerImgSrc = frontEight(data, '00010003')
      let bannerImgBtn = frontEight(data, '00010004')

      let bannerImgArray = {title: '', describe: '', src: '', btn: '', url: ''}
      let bannerImgArrayId = {title: '', describe: '', src: '', btn: ''}
      for (let i = 0, j = bannerImgTitle.length; i < j; i++) {
        bannerImgArray.title = bannerImgTitle[i].content
        bannerImgArray.describe = bannerImgDescribe[i].content
        bannerImgArray.src = bannerImgSrc[i].content
        bannerImgArray.btn = bannerImgBtn[i].content
        bannerImgArray.url = bannerImgBtn[i].url
        this.bannerImgArray.push(bannerImgArray)
        bannerImgArray = {title: '', describe: '', src: '', btn: '', url: ''}
        bannerImgArrayId.title = 'e' + bannerImgTitle[i].code
        bannerImgArrayId.describe = 'e' + bannerImgDescribe[i].code
        bannerImgArrayId.src = 'e' + bannerImgSrc[i].code
        bannerImgArrayId.btn = 'e' + bannerImgBtn[i].code
        this.bannerImgArrayId.push(bannerImgArrayId)
        bannerImgArrayId = {title: '', describe: '', src: '', btn: ''}
      }
    },
    // 热门产品元素
    hotProductElement(data) {
      let hotProductTitle = frontEight(data, '00010005')
      let hotProductHot = frontEight(data, '00010006')
      let hotProductTag = frontEight(data, '00010007')
      let hotProductPrice = frontEight(data, '00010008')
      let hotProductUnit = frontEight(data, '00010009')
      let hotProductDiscounts = frontEight(data, '00010010')
      let hotProductImg0 = frontEight(data, '00010011')
      let hotProductImg1 = frontEight(data, '00010012')

      let hotProduct = {
        title: '',
        hot: '',
        tag: [],
        price: '',
        unit: '',
        discounts: '',
        img: [],
        url: ''
      }
      let hotProductId = {
        title: '',
        hot: '',
        tag: '',
        price: '',
        unit: '',
        discounts: '',
        img: []
      }
      for (let i = 0, j = hotProductTitle.length; i < j; i++) {
        hotProduct.title = hotProductTitle[i].content
        hotProduct.hot = hotProductHot[i].content
        hotProduct.tag = splitStr(hotProductTag[i].content)
        hotProduct.price = hotProductPrice[i].content
        hotProduct.unit = hotProductUnit[i].content
        hotProduct.discounts = hotProductDiscounts[i].content
        hotProduct.img = [hotProductImg0[i].content, hotProductImg1[i].content]
        hotProduct.url = hotProductImg0[i].url
        this.hotProduct.push(hotProduct)
        hotProduct = {
          title: '',
          hot: '',
          tag: [],
          price: '',
          unit: '',
          discounts: '',
          img: [],
          url: ''
        }

        hotProductId.title = 'e' + hotProductTitle[i].code
        hotProductId.hot = 'e' + hotProductHot[i].code
        hotProductId.tag = 'e' + hotProductTag[i].code
        hotProductId.price = 'e' + hotProductPrice[i].code
        hotProductId.unit = 'e' + hotProductUnit[i].code
        hotProductId.discounts = 'e' + hotProductDiscounts[i].code
        hotProductId.img = ['e' + hotProductImg0[i].code, 'e' + hotProductImg1[i].code]
        this.hotProductId.push(hotProductId)
        hotProductId = {
          title: '',
          hot: '',
          tag: '',
          price: '',
          unit: '',
          discounts: '',
          img: []
        }
      }
    },
    // 专网组合产品推荐元素
    recommendationElement(data) {
      let recommendationTitle = frontEight(data, '00010013')
      let recommendationImg0 = frontEight(data, '00010014')
      let recommendationImg1 = frontEight(data, '00010015')
      let recommendationPreTitle = frontEight(data, '00010016')
      let recommendationPreDescribe = frontEight(data, '00010017')
      let commodityTitle = frontEight(data, '00010018')
      let commodityDescribe = frontEight(data, '00010019')
      let commodityPrice = frontEight(data, '00010020')
      let commodityUnit = frontEight(data, '00010021')
      let commodityBtn = frontEight(data, '00010022')

      let recommendation = {
        title: '',
        img: [],
        preTitle: '',
        preDescribe: '',
        commodity: []
      }
      let recommendationId = {
        title: '',
        img: [],
        preTitle: '',
        preDescribe: '',
        commodity: []
      }
      for (let i = 0, j = recommendationTitle.length; i < j; i++) {
        recommendation.title = recommendationTitle[i].content
        recommendation.img = [
          recommendationImg0[i].content,
          recommendationImg1[i].content
        ]
        recommendation.preTitle = recommendationPreTitle[i].content
        recommendation.preDescribe = recommendationPreDescribe[i].content
        recommendationId.title = 'e' + recommendationTitle[i].code
        recommendationId.img = [
          'e' + recommendationImg0[i].code,
          'e' + recommendationImg1[i].code
        ]
        recommendationId.preTitle = 'e' + recommendationPreTitle[i].code
        recommendationId.preDescribe = 'e' + recommendationPreDescribe[i].code

        let commodityTitleEnd = parentCode(commodityTitle, recommendationTitle[i].code)
        let commodityDescribeEnd = parentCode(
          commodityDescribe,
          recommendationTitle[i].code
        )
        let commodityPriceEnd = parentCode(commodityPrice, recommendationTitle[i].code)
        let commodityUnitEnd = parentCode(commodityUnit, recommendationTitle[i].code)
        let commodityBtnEnd = parentCode(commodityBtn, recommendationTitle[i].code)

        let commodity = {title: '', tag: '', price: '', unit: '', btn: '', url: ''}
        let commodityId = {title: '', tag: '', price: '', unit: '', btn: ''}
        for (let m = 0, n = commodityTitleEnd.length; m < n; m++) {
          commodity.title = commodityTitleEnd[m].content
          commodity.tag = splitStr(commodityDescribeEnd[m].content)
          commodity.price = commodityPriceEnd[m].content
          commodity.unit = commodityUnitEnd[m].content
          commodity.btn = commodityBtnEnd[m].content
          commodity.url = commodityBtnEnd[m].url
          commodityId.title = 'e' + commodityTitleEnd[m].code
          commodityId.tag = 'e' + commodityDescribeEnd[m].code
          commodityId.price = 'e' + commodityPriceEnd[m].code
          commodityId.unit = 'e' + commodityUnitEnd[m].code
          commodityId.btn = 'e' + commodityBtnEnd[m].code

          recommendation.commodity.push(commodity)
          recommendationId.commodity.push(commodityId)
          commodity = {title: '', tag: '', price: '', unit: '', btn: '', url: ''}
          commodityId = {title: '', tag: '', price: '', unit: '', btn: ''}
        }

        this.recommendation.push(recommendation)
        this.recommendationId.push(recommendationId)
        recommendation = {
          title: '',
          img: [],
          preTitle: '',
          preDescribe: '',
          commodity: []
        }
        recommendationId = {
          title: '',
          img: [],
          preTitle: '',
          preDescribe: '',
          commodity: []
        }
      }
    },
    // 精选方案元素
    selectionSchemeElement(data) {
      let schemeMenuTab = frontEight(data, '00010023')
      let schemeMenuTitle = frontEight(data, '00010024')
      let schemeMenuValue = frontEight(data, '00010025')
      let schemeMenuImg = frontEight(data, '00010026')

      let schemeMenu = {tab: '', title: '', value: '', img: '', link: ''}
      let schemeMenuId = {tab: '', title: '', value: '', img: '', link: ''}
      for (let i = 0, j = schemeMenuTab.length; i < j; i++) {
        schemeMenu.tab = schemeMenuTab[i].content
        schemeMenu.title = schemeMenuTitle[i].content
        schemeMenu.value = splitStr(schemeMenuValue[i].content)
        schemeMenu.img = schemeMenuImg[i].content
        schemeMenu.link = schemeMenuImg[i].url
        schemeMenuId.tab = 'e' + schemeMenuTab[i].code
        schemeMenuId.title = 'e' + schemeMenuTitle[i].code
        schemeMenuId.value = 'e' + splitStr(schemeMenuValue[i].code)
        schemeMenuId.img = 'e' + schemeMenuImg[i].code

        this.schemeMenu.push(schemeMenu)
        this.schemeMenuId.push(schemeMenuId)
        schemeMenu = {tab: '', title: '', value: '', img: '', link: ''}
        schemeMenuId = {tab: '', title: '', value: '', img: '', link: ''}
      }
    },
    // 行业资讯元素
    informationElement(data) {
      let informationTitle = frontEight(data, '00010027')
      let informationContainer = frontEight(data, '00010028')
      let informationTime = frontEight(data, '00010029')
      let informationImg = frontEight(data, '00010030')

      let information = {title: '', container: '', time: '', img: '', url: ''}
      let informationId = {title: '', container: '', time: '', img: '', url: ''}
      for (let i = 0, j = informationTitle.length; i < j; i++) {
        information.title = informationTitle[i].content
        information.container = informationContainer[i].content
        information.time = informationTime[i].content
        information.img = informationImg[i].content
        information.url = informationImg[i].url

        informationId.title = 'e' + informationTitle[i].code
        informationId.container = 'e' + informationContainer[i].code
        informationId.time = 'e' + informationTime[i].code
        informationId.img = 'e' + informationImg[i].code

        this.information.push(information)
        this.informationId.push(informationId)

        information = {title: '', container: '', time: '', img: '', url: ''}
        informationId = {title: '', container: '', time: '', img: '', url: ''}
      }
    },
    // 标题
    containerTitleElement(data) {
      this.containerElement1.title = frontEight(data, '00010031')[0].content
      this.containerElement1.describe = frontEight(data, '00010032')[0].content
      this.containerElement2.title = frontEight(data, '00010033')[0].content
      this.containerElement2.describe = frontEight(data, '00010034')[0].content
      this.containerElement3.title = frontEight(data, '00010035')[0].content
      this.containerElement3.describe = frontEight(data, '00010036')[0].content
      this.containerElement4.title = frontEight(data, '00010037')[0].content
      this.containerElement4.describe = frontEight(data, '00010038')[0].content
      this.moreProducts.title = frontEight(data, '00010039')[0].content
      this.moreProducts.url = frontEight(data, '00010039')[0].url
      this.moreInformation.title = frontEight(data, '00010040')[0].content
      this.moreInformation.url = frontEight(data, '00010040')[0].url
      this.containerElement1Id.title = 'e' + frontEight(data, '00010031')[0].code
      this.containerElement1Id.describe = 'e' + frontEight(data, '00010032')[0].code
      this.containerElement2Id.title = 'e' + frontEight(data, '00010033')[0].code
      this.containerElement2Id.describe = 'e' + frontEight(data, '00010034')[0].code
      this.containerElement3Id.title = 'e' + frontEight(data, '00010035')[0].code
      this.containerElement3Id.describe = 'e' + frontEight(data, '00010036')[0].code
      this.containerElement4Id.title = 'e' + frontEight(data, '00010037')[0].code
      this.containerElement4Id.describe = 'e' + frontEight(data, '00010038')[0].code
    },
    // //关闭智能导购引导
    // closeOperationGuidance() {
    //   this.setSmartGuideInfo({
    //     showOperationGuidance: false,
    //     showGuideFlow: this.smartGuideInfo.showGuideFlow,
    //     showShoppingGuide: this.smartGuideInfo.showShoppingGuide
    //   })
    // },
    // banner轮播
    bannerShuff() {
      this.timer1 = setInterval(this.autoPlay, 5000)
    },
    autoPlay() {
      this.bannerNum++
      if (this.bannerImgArray.length == this.bannerNum) {
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

    bannerIndex(index) {
      this.bannerNum = index
    },
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
      this.bannerShuff()
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
