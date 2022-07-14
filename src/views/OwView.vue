<template>
  <div class="about">
    <h1>오퍼월 웹뷰 테스트</h1>
    <ul>
      <li v-for="ad in ads" v-bind:key="ad.id" @click="showAd(ad)">
        {{ ad.title }}
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import { Ofw } from "adison-offerwall-web-sdk/lib/app";

@Component({
  data() {
    return {
      ads: [],
    };
  },
  components: {
    HelloWorld,
  },
  methods: {
    register: function (ad, adView) {
      Ofw.registerNativeAd(ad, adView);
    },
    showAd: function (ad) {
      Ofw.showOfferallAd(ad);
    },
  },
  watch: {
    ads: {
      handler(newVal, oldVal) {
        // Do something when the DOM changes!
        newVal.forEach((ad: any) => {
          Ofw.impression(ad.id);
        });
      },
      deep: true,
    },
  },
  mounted() {
    Ofw.init({
      android: "f7Pq9yXSooZPHRLv9M5rXQhG",
      ios: "SFEyJwarfNQGKK4zFrPYQ1nb",
    });

    Ofw.loadAds({ inventory: "trend_tab", filter: ["test", "t"] }, 5)
      .then((ads) => {
        this.$data.ads = ads;
        // this.$set(this.$data.ads, ads, this.$refs.ads);
      })
      .catch((e) => {
        console.log(e);
      });
  },
})
export default class HomeView extends Vue {}
</script>
