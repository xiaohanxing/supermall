<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
        <home-swiper :banners="banners"></home-swiper>
        <recommend-view :recommends="recommends"></recommend-view>
        <feature-view/>
        <tab-control class="tab-control" :titles="['流行','新款','精选']"/>
        <div>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br>1<br></div>
    </div>
</template>

<script>

    import HomeSwiper from './childComps/HomeSwiper'
    import RecommendView from './childComps/RecommendView'
    import FeatureView from './childComps/FeatureView'

    import NavBar from 'components/common/navbar/NavBar'
    import TabControl from 'components/content/tabControl/TabControl'

    import {getHomeMultidata,getHomeGoods} from 'network/home'


    export default {
    name: 'Home',
    data() {
        return {
            banners: [],
            recommends: [],
            goods: {
                'pop': {page: 0, list: []},
                'news': {page: 0, list: []},
                'sell': {page: 0, list: []},
            }
        }
    },
    components: {
        HomeSwiper,
        RecommendView,
        FeatureView,
        NavBar,
        TabControl
    },
    created() {
        this.init()
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
    },
    methods: {
         init() {
           getHomeMultidata().then(res => {
             console.log(res);
             this.banners = res.data.banner.list;
             this.recommends = res.data.recommend.list;
           })
         },
         getHomeGoods(type) {
           const page = this.goods[type].page + 1
           getHomeGoods(type,page).then(res => {
                 this.goods[type].list.push(...res.data.list) 
                 this.goods[type].page += 1
             })
         }
    }
}
</script>

<style scoped>
  .home {
      padding-top: 44px;
  }
  .home-nav {
      background-color: var(--color-tint);
      color: white;

      position: fixed;
      left: 0;
      right: 0;
      top: 0;
      z-index: 9;
  }
  .tab-control {
      position: sticky;
      top: 100px;
  }
</style>