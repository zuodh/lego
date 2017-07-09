<template>
  <div class="profile_page">
    <head-top go-back='true' :head-title="profiletitle"></head-top>
    <section class="dowload_container">
      <img src='../../images/pic3.png' class="logo_img" style="width:100%">
      <img src='../../images/pic2.png' style="width:100%">
      <div style="background-color: #ffffff">
        <p style="font-size:small">洗车券、服务券使用说明：</p>
        <p style="font-size:small">1、20元面值洗车券适用于普洗，无需另外支付现金；如需其他
          清洗，可累计使用洗车券，面值等同现金，但不找零；</p>
        <p style="font-size:small">2、服务券面值为10元，效力等同现金，可累计使用，可享受洗
          车、保养服务、维修服务、安装服务等平台内所有提供的服务；</p>
        <p style="font-size:small">3、所有卡、券可在Car  Boy平台及所有门店通用，并无时效限制</p>
        <p style="font-size:small">4、卡、券可以通过注册获赠，也可以通过购买商品（服务）获
          赠，亦可通过现金购买；</p>
      </div>
    </section>
    <foot-guide></foot-guide>
    <transition name="router-slid" mode="out-in">
      <router-view></router-view>
    </transition>
  </div>
</template>

<script>
  import headTop from 'src/components/header/head'
  import footGuide from 'src/components/footer/footGuide'
  import {mapState, mapMutations} from 'vuex'
  import {imgBaseUrl} from 'src/config/env'
  import {getImgPath} from 'src/components/common/mixin'

  export default {
    data(){
      return{
        profiletitle: '关于我们',
        username: '登录/注册',           //用户名
        resetname: '',
        mobile: '暂无绑定手机号',             //电话号码
        balance: 0,            //我的余额
        count : 0,             //优惠券个数
        pointNumber : 0,       //积分数
        avatar: '',             //头像地址
        imgBaseUrl,
      }
    },
    mounted(){
      this.initData();
    },
    mixins: [getImgPath],
    components:{
      headTop,
      footGuide,
    },

    computed:{
      ...mapState([
        'userInfo',
      ]),
      //后台会返回两种头像地址格式，分别处理
      imgpath:function () {
        let path;
        if(this.avatar.indexOf('/') !==-1){
          path = imgBaseUrl +　this.avatar;
        }else{
          path = this.getImgPath(this.avatar)
        }
        this.SAVE_AVANDER(path);
        return path;
      }
    },

    methods:{
      ...mapMutations([
        'SAVE_AVANDER'
      ]),
      initData(){
        if (this.userInfo && this.userInfo.user_id) {
          this.avatar = this.userInfo.avatar;
          this.username = this.userInfo.username;
          this.mobile = this.userInfo.mobile || '暂无绑定手机号';
          this.balance = this.userInfo.balance;
          this.count = this.userInfo.gift_amount;
          this.pointNumber = this.userInfo.point;
        }else{
          this.username = '登录/注册';
          this.mobile = '暂无绑定手机号';
        }
      },
    },
    watch: {
      userInfo: function (value){
        this.initData()
      }
    }
  }

</script>

<style lang="scss" scoped>
  @import 'src/style/mixin';

  .profile_page{
    p, span{
      font-family: Helvetica Neue,Tahoma,Arial;
    }
  }
  .profile-number{
    padding-top:1.95rem;
    .profile-link{
      display:block;
      display:flex;
      box-align: center;
      -webkit-box-align: center;
      -ms-flex-align: center;
      align-items: center;
      background:$blue;
      padding: .666667rem .6rem;
      .privateImage{
        display:inline-block;
        @include wh(2.5rem,2.5rem);
        border-radius:50%;
        vertical-align:middle;
        .privateImage-svg{
          background:$fc;
          border-radius:50%;
          @include wh(2.5rem,2.5rem);
        }
      }
      .user-info{
        margin-left:.48rem;
        -webkit-box-flex: 1;
        -ms-flex-positive: 1;
        flex-grow: 1;
        p{
          font-weight:700;
          @include sc(.8rem,$fc);
          .user-icon{
            @include wh(0.5rem,0.75rem);
            display:inline-block;
            vertical-align:middle;
            line-height:0.75rem;
            .icon-mobile{
              @include wh(100%,100%);
            }
          }
          .icon-mobile-number{
            display:inline-block;
            @include sc(.57333rem,$fc);

          }
        }

      }
      .arrow{
        @include wh(.46667rem,.98rem);
        display:inline-block;
        svg{
          @include wh(100%,100%);
        }
      }
    }
  }
  .info-data{
    width:100%;
    background:$fc;
    box-sizing: border-box;
    ul{
      .info-data-link{
        float:left;
        width:33.33%;
        display:inline-block;
        border-right:1px solid #f1f1f1;
        span{
          display:block;
          width:100%;
          text-align:center;
        }
        .info-data-top{
          @include sc(.55rem,#333);
          padding: .853333rem 0 .453333rem;
          b{
            display:inline-block;
            @include sc(1.2rem,#f90);
            font-weight:700;
            line-height:1rem;
            font-family: Helvetica Neue,Tahoma;
          }
        }
        .info-data-bottom{
          @include sc(.57333rem,#666);
          font-weight:400;
          padding-bottom:.453333rem;

        }
      }
      .info-data-link:nth-of-type(2){
        .info-data-top{
          b{
            color:#ff5f3e;
          }
        }

      }
      .info-data-link:nth-of-type(3){
        border:0;
        .info-data-top{
          b{
            color:#6ac20b;
          }
        }
      }
    }
  }
  .profile-1reTe{
    margin-top:.4rem;
    background:$fc;
    .myorder{
      padding-left:1.6rem;
      display:flex;
      align-items: center;
      aside{
        @include wh(.7rem,.7rem);
        margin-left:-.866667rem;
        margin-right:.266667rem;
        display:flex;
        align-items: center;
        svg{
          @include wh(100%,100%);
        }
      }
      .myorder-div{
        width:100%;
        border-bottom:1px solid #f1f1f1;
        padding:.433333rem .266667rem .433333rem 0;
        @include sc(.7rem,#333);
        display:flex;
        justify-content:space-between;
        span{
          display:block;
        }
        .myorder-divsvg{
          @include wh(.46667rem,.466667rem);
          svg{
            @include wh(100%,100%);
          }
        }
      }
    }
    .myorder:nth-of-type(3) .myorder-div{
      border:0;
    }
  }
  .router-slid-enter-active, .router-slid-leave-active {
    transition: all .4s;
  }
  .router-slid-enter, .router-slid-leave-active {
    transform: translate3d(2rem, 0, 0);
    opacity: 0;
  }
  .dowload_container{
    .logo_img{
      border-radius: 0rem;
      margin-top: 2rem;
    }
    p{
      @include sc(.8rem, #666);
      margin-bottom: 0.8rem;
    }
    .determine{
      background-color: #FFFFFF;
      @include sc(.7rem, #fff);
      text-align: center;
      margin: 0 .7rem;
      line-height: 1.8rem;
      border-radius: 0.2rem;
      margin-top: 0.5rem;
    }
  }
</style>
