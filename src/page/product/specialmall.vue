<template>
  <div>
    <head-top head-title="特价商城" go-back='true'></head-top>
    <div><img src="../../images/chexing.png" style="margin-top: 40px"/><img style="margin-top: 40px;" src="../../images/chezi.png"/></div>
    <nav class="msite_nav">
      <div class="swiper-container" v-if="foodTypes.length">
        <div class="swiper-wrapper">
          <div class="swiper-slide food_types_container" v-for="(item, index) in foodTypes" :key="index">
            <router-link :to="{path: '/food', query: {geohash, title: foodItem.title, restaurant_category_id: getCategoryId(foodItem.link)}}" v-for="foodItem in item" :key="foodItem.id" class="link_to_food">
              <figure>
                <img :src="imgBaseUrl + foodItem.image_url">
                <figcaption>{{foodItem.title}}</figcaption>
              </figure>
            </router-link>
          </div>
          <div class="swiper-pagination"></div>
        </div>
      </div>
      <img src="../../images/fl.svg" class="fl_back animation_opactiy" v-else>
      <div style="margin-top: 20px;"><img src="../../images/lxsp.png" style="margin-left: 25px"/><img style="margin-left: 20px;" src="../../images/lxbh.png"/></div>
      <!--<div>-->
          <!--<span  style="margin-left: 50px;font-size: medium">旅行零食</span><span style="margin-left:70px;font-size: medium">旅行百货</span>-->
      <!--</div>-->
    </nav>
    <div >
      <!--<header class="shop_header">-->
      <!--<svg class="shop_icon">-->
      <!--<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#shop"></use>-->
      <!--</svg>-->
      <!--<span class="shop_header_title">附近商家</span>-->
      <!--</header>-->
      <!--<shop-list v-if="hasGetData" :geohash="geohash"></shop-list>-->
    </div>
    <foot-guide></foot-guide>
  </div>
</template>

<script>
  import {mapMutations} from 'vuex'
  // import {imgBaseUrl} from 'src/config/env'
  import headTop from 'src/components/header/head'
  import footGuide from 'src/components/footer/footGuide'
  import shopList from 'src/components/common/shoplist'
  import {msiteAdress, msiteFoodTypes, cityGuess} from 'src/service/getData'
  import 'src/plugins/swiper.min.js'
  import 'src/style/swiper.min.css'

  export default {
    data(){
      return {
        geohash: '', // city页面传递过来的地址geohash
        msietTitle: '请选择地址...', // msiet页面头部标题
        foodTypes: [], // 食品分类列表
        hasGetData: false, //是否已经获取地理位置数据，成功之后再获取商铺列表信息
        imgBaseUrl: 'http://120.26.106.142', //图片域名地址
      }
    },
    async beforeMount(){
      if (!this.$route.query.geohash) {
        const address = await cityGuess();
        this.geohash = address.latitude + ',' + address.longitude;
      }else{
        this.geohash = this.$route.query.geohash
      }
      //保存geohash 到vuex
      this.SAVE_GEOHASH(this.geohash);
      //获取位置信息
      let res = await msiteAdress(this.geohash);
      this.msietTitle = res.name;
      // 记录当前经度纬度
      this.RECORD_ADDRESS(res);

      this.hasGetData = true;
    },
    mounted(){
      //获取导航食品类型列表
      msiteFoodTypes(this.geohash).then(res => {
        let resLength = res.length;
        let resArr =[{"id": 20,"is_in_serving": true,"description": "苦了累了，来点甜的","title": "刹车片","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu751c%5Cu54c1%5Cu996e%5Cu54c1%22%2C%22complex_category_ids%22%3A%5B240%2C241%2C242%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A239%2C%22name%22%3A%22%5Cu751c%5Cu54c1%5Cu996e%5Cu54c1%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E7%94%9C%E5%93%81%E9%A5%AE%E5%93%81&animation_type=1&is_need_mark=0&banner_type=","image_url": "/scp.png","icon_url": "","title_color": "","__v": 0},{"id": 15,"is_in_serving": true,"description": "附近美食一网打尽","title": "刹车盘","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu7f8e%5Cu98df%22%2C%22complex_category_ids%22%3A%5B207%2C220%2C233%2C260%5D%2C%22is_show_all_category%22%3Afalse%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E7%BE%8E%E9%A3%9F&animation_type=1&is_need_mark=0&banner_type=","image_url": "/scpan.png","icon_url": "","title_color": "","__v": 0},{"id": 92,"is_in_serving": true,"description": "准时必达，超时赔付","title": "轮胎","link": "eleme://restaurants?filter_key=%7B%22support_ids%22%3A%5B9%5D%2C%22activities%22%3A%5B%7B%22id%22%3A9%2C%22name%22%3A%22%5Cu51c6%5Cu65f6%5Cu8fbe%22%2C%22icon_name%22%3A%22%5Cu51c6%22%2C%22icon_color%22%3A%22E8842D%22%2C%22is_need_filling%22%3A0%2C%22is_multi_choice%22%3A1%2C%22filter_value%22%3A9%2C%22filter_key%22%3A%22support_ids%22%2C%22description%22%3A%22%5Cu51c6%5Cu65f6%5Cu8fbe%22%7D%5D%7D&target_name=%E5%87%86%E6%97%B6%E8%BE%BE&animation_type=1&is_need_mark=0&banner_type=","image_url": "/luntai.png","icon_url": "","title_color": "","__v": 0},{"id": 403297,"is_in_serving": true,"description": "大胆尝鲜，遇见惊喜","title": "轮毂","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu65b0%5Cu5e97%5Cu7279%5Cu60e0%22%2C%22complex_category_ids%22%3A%5B207%2C220%2C233%2C239%2C244%2C248%2C252%2C260%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22support_ids%22%3A%5B-1%5D%2C%22activities%22%3A%5B%5D%7D&target_name=%E6%96%B0%E5%BA%97%E7%89%B9%E6%83%A0&animation_type=1&is_need_mark=0&banner_type=","image_url": "/lungu.png","icon_url": "","title_color": "","__v": 0},{"id": 9,"is_in_serving": true,"description": "内心小公举，一直被宠爱","title": "减震器","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu9c9c%5Cu82b1%5Cu86cb%5Cu7cd5%22%2C%22complex_category_ids%22%3A%5B249%2C250%2C251%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A248%2C%22name%22%3A%22%5Cu9c9c%5Cu82b1%5Cu86cb%5Cu7cd5%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E9%B2%9C%E8%8A%B1%E8%9B%8B%E7%B3%95&animation_type=1&is_need_mark=0&banner_type=","image_url": "/jzq.png","icon_url": "","title_color": "","__v": 0},{"id": 289,"is_in_serving": true,"description": "老字号，好味道","title": "车灯","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5305%5Cu5b50%5Cu7ca5%5Cu5e97%22%2C%22complex_category_ids%22%3A%5B215%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%8C%85%E5%AD%90%E7%B2%A5%E5%BA%97&animation_type=1&is_need_mark=0&banner_type=","image_url": "/chedeng.png","icon_url": "","title_color": "","__v": 0},{"id": 10,"is_in_serving": true,"description": "足不出户，便利回家","title": "电瓶","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5546%5Cu8d85%5Cu4fbf%5Cu5229%22%2C%22complex_category_ids%22%3A%5B254%2C255%2C256%2C257%2C258%2C271%2C272%2C273%2C274%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A252%2C%22name%22%3A%22%5Cu5546%5Cu5e97%5Cu8d85%5Cu5e02%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%95%86%E8%B6%85%E4%BE%BF%E5%88%A9&animation_type=1&is_need_mark=0&banner_type=","image_url": "/dinaping.png","icon_url": "","title_color": "","__v": 0},{"id": 1,"is_in_serving": true,"description": "0元早餐0起送，每天都有新花样。","title": "车载电器","link": "eleme://web?url=https%3A%2F%2Fzaocan.ele.me&target_name=%E9%A2%84%E8%AE%A2%E6%97%A9%E9%A4%90&animation_type=1&is_need_mark=&banner_type=","image_url": "/czdq.png","icon_url": "","title_color": "","__v": 0},{"id": 8,"is_in_serving": true,"description": "一天变女神","title": "雨刮器","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu679c%5Cu852c%5Cu751f%5Cu9c9c%22%2C%22complex_category_ids%22%3A%5B245%2C246%2C247%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A244%2C%22name%22%3A%22%5Cu679c%5Cu852c%5Cu751f%5Cu9c9c%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E6%9E%9C%E8%94%AC%E7%94%9F%E9%B2%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/ygq.png","icon_url": "","title_color": "","__v": 0},{"id": 236,"is_in_serving": true,"description": "大口大口把你吃掉","title": "其他","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu6c49%5Cu5821%22%2C%22complex_category_ids%22%3A%5B212%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E6%B1%89%E5%A0%A1%E8%96%AF%E6%9D%A1&animation_type=1&is_need_mark=0&banner_type=","image_url": "/other.png","icon_url": "","title_color": "","__v": 0},{"id": 287,"is_in_serving": true,"description": "西餐始祖，欧洲的味道","title": "机油","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu62ab%5Cu8428%5Cu610f%5Cu9762%22%2C%22complex_category_ids%22%3A%5B211%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A260%2C%22name%22%3A%22%5Cu5f02%5Cu56fd%5Cu6599%5Cu7406%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E6%8A%AB%E8%90%A8%E6%84%8F%E9%9D%A2&animation_type=1&is_need_mark=0&banner_type=","image_url": "/jiyou.png","icon_url": "","title_color": "","__v": 0},{"id": 285,"is_in_serving": true,"description": "寿司定食，泡菜烤肉","title": "保养剂","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu65e5%5Cu97e9%5Cu6599%5Cu7406%22%2C%22complex_category_ids%22%3A%5B229%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A260%2C%22name%22%3A%22%5Cu5f02%5Cu56fd%5Cu6599%5Cu7406%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E6%97%A5%E9%9F%A9%E6%96%99%E7%90%86&animation_type=1&is_need_mark=0&banner_type=","image_url": "/byj.png","icon_url": "","title_color": "","__v": 0},{"id": 225,"is_in_serving": true,"description": "有菜有肉，营养均衡","title": "清洗剂","link": "eleme://restaurants?filter_key=%7B%22activity_types%22%3A%5B3%5D%2C%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu7b80%5Cu9910%22%2C%22complex_category_ids%22%3A%5B209%2C212%2C215%2C265%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%7B%22id%22%3A3%2C%22name%22%3A%22%5Cu4e0b%5Cu5355%5Cu7acb%5Cu51cf%22%2C%22icon_name%22%3A%22%5Cu51cf%22%2C%22icon_color%22%3A%22f07373%22%2C%22is_need_filling%22%3A1%2C%22is_multi_choice%22%3A0%2C%22filter_value%22%3A3%2C%22filter_key%22%3A%22activity_types%22%7D%5D%7D&target_name=%E7%AE%80%E9%A4%90&animation_type=1&is_need_mark=0&banner_type=","image_url": "/qjj.png","icon_url": "","title_color": "","__v": 0},{"id": 65,"is_in_serving": true,"description": "","title": "制冷液","link": "eleme://restaurants?filter_key=%7B%22activities%22%3A%5B%7B%22filter_key%22%3A%22tags%22%2C%22filter_value%22%3A0%7D%5D%7D&target_name=%E5%9C%9F%E8%B1%AA%E6%8E%A8%E8%8D%90&animation_type=1&is_need_mark=0&banner_type=","image_url": "/zly.png","icon_url": "","title_color": "","__v": 0},{"id": 286,"is_in_serving": true,"description": "","title": "刹车油","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu9ebb%5Cu8fa3%5Cu70eb%22%2C%22complex_category_ids%22%3A%5B214%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A207%2C%22name%22%3A%22%5Cu5feb%5Cu9910%5Cu4fbf%5Cu5f53%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E9%BA%BB%E8%BE%A3%E7%83%AB&animation_type=1&is_need_mark=0&banner_type=","image_url": "/scy.png","icon_url": "","title_color": "","__v": 0},{"id": 288,"is_in_serving": true,"description": "无辣不欢","title": "检测保养","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5ddd%5Cu6e58%5Cu83dc%22%2C%22complex_category_ids%22%3A%5B221%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A220%2C%22name%22%3A%22%5Cu7279%5Cu8272%5Cu83dc%5Cu7cfb%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%B7%9D%E6%B9%98%E8%8F%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/jcby.png","icon_url": "","title_color": "","__v": 0},{"id": 1000,"is_in_serving": true,"description": "无辣不欢","title": "喷漆贴膜","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5ddd%5Cu6e58%5Cu83dc%22%2C%22complex_category_ids%22%3A%5B221%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A220%2C%22name%22%3A%22%5Cu7279%5Cu8272%5Cu83dc%5Cu7cfb%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%B7%9D%E6%B9%98%E8%8F%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/tqtm.png","icon_url": "","title_color": "","__v": 0},{"id": 1001,"is_in_serving": true,"description": "无辣不欢","title": "换胎维修","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5ddd%5Cu6e58%5Cu83dc%22%2C%22complex_category_ids%22%3A%5B221%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A220%2C%22name%22%3A%22%5Cu7279%5Cu8272%5Cu83dc%5Cu7cfb%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%B7%9D%E6%B9%98%E8%8F%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/htwx.png","icon_url": "","title_color": "","__v": 0},{"id": 1002,"is_in_serving": true,"description": "无辣不欢","title": "洗车美容","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5ddd%5Cu6e58%5Cu83dc%22%2C%22complex_category_ids%22%3A%5B221%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A220%2C%22name%22%3A%22%5Cu7279%5Cu8272%5Cu83dc%5Cu7cfb%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%B7%9D%E6%B9%98%E8%8F%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/xcmr.png","icon_url": "","title_color": "","__v": 0},{"id": 1003,"is_in_serving": true,"description": "无辣不欢","title": "车饰改装","link": "eleme://restaurants?filter_key=%7B%22category_schema%22%3A%7B%22category_name%22%3A%22%5Cu5ddd%5Cu6e58%5Cu83dc%22%2C%22complex_category_ids%22%3A%5B221%5D%2C%22is_show_all_category%22%3Atrue%7D%2C%22restaurant_category_id%22%3A%7B%22id%22%3A220%2C%22name%22%3A%22%5Cu7279%5Cu8272%5Cu83dc%5Cu7cfb%22%2C%22sub_categories%22%3A%5B%5D%2C%22image_url%22%3A%22%22%7D%2C%22activities%22%3A%5B%5D%7D&target_name=%E5%B7%9D%E6%B9%98%E8%8F%9C&animation_type=1&is_need_mark=0&banner_type=","image_url": "/csgz.png","icon_url": "","title_color": "","__v": 0}];
        let foodArr = [];
        for (let i = 0, j = 0; i < resLength; i += 20, j++) {
          foodArr[j] = resArr.splice(0, 20);
        }
        this.foodTypes = foodArr;
      }).then(() => {
        //初始化swiper
        new Swiper('.swiper-container', {
          pagination: '.swiper-pagination',
          loop: true
        });
      })
    },
    components: {
      headTop,
      shopList,
      footGuide,
    },
    computed: {

    },
    methods: {
      ...mapMutations([
        'RECORD_ADDRESS', 'SAVE_GEOHASH'
      ]),
      // 解码url地址，求去restaurant_category_id值
      getCategoryId(url){
        let urlData = decodeURIComponent(url.split('=')[1].replace('&target_name',''));
        if (/restaurant_category_id/gi.test(urlData)) {
          return JSON.parse(urlData).restaurant_category_id.id
        }else{
          return ''
        }
      }
    },
    watch: {

    }
  }

</script>

<style lang="scss" scoped>
  @import 'src/style/mixin';
  .link_search{
    left: .8rem;
    @include wh(.9rem, .9rem);
    @include ct;
  }
  .msite_title{
    @include center;
    width: 50%;
    color: #fff;
    text-align: center;
    margin-left: -0.5rem;
    .title_text{
      @include sc(0.8rem, #fff);
      text-align: center;
      display: block;
    }
  }
  .msite_nav{
    padding-top: 1.1rem;
    background-color: #fff;
    border-bottom: 0.025rem solid $bc;
    height: 100%;
    .swiper-container{
      @include wh(100%, auto);
      padding-bottom: 0.3rem;
      .swiper-pagination{
        bottom: 0.2rem;
      }
    }
    .fl_back{
      @include wh(100%, 100%);
    }
  }
  .food_types_container{
    display:flex;
    flex-wrap: wrap;
    .link_to_food{
      width: 20%;
      height: 30%;
      padding: 0.3rem 0rem;
      @include fj(center);
      figure{
        img{
          margin-bottom: 0.3rem;
          @include wh(1.8rem, 1.8rem);
        }
        figcaption{
          text-align: center;
          @include sc(0.55rem, #666);
        }
      }
    }
  }
  .shop_list_container{
    margin-top: .4rem;
    border-top: 0.025rem solid $bc;
    background-color: #fff;
    .shop_header{
      .shop_icon{
        fill: #999;
        margin-left: 0.6rem;
        vertical-align: middle;
        @include wh(0.6rem, 0.6rem);
      }
      .shop_header_title{
        color: #999;
        @include font(0.55rem, 1.6rem);
      }
    }
  }

</style>
