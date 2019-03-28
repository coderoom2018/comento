<template>
  <div class="myAds">
    <h1>Ads</h1>
    <div class="myAds_container" v-for="ad in ads" v-bind:key="ad.no">
      <div class="myAd">
        <img class="myAd_image" v-bind:src=ad.img >
        <div class="myAd_main">
          <div class="myAd_main_title">{{ ad.title }}</div>
          <div class="myAd_main_contents">{{ ad.contents }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Ads",
  data: function() {
    return {
      ads: this.ads,
    };
  },
  props: ['ads'],
  mounted: function() {
    // this.getAds();
  },
  methods: {
    getAds: function() {
      const URI = "http://comento.cafe24.com/ads.php";
      const page = 1;
      const limit = 2;

      this.$http
        .get(`${URI}?page=${page}&limit=${limit}`)
        .then(result => {
          console.log("Ads: ", result.data.list);
          let list = result.data.list
          list.map(ad => {
            ad.img = `http://comento.cafe24.com/public/images/${ad.img}`
          })
          this.ads = list;
        })
        .catch(error => {
          console.log(error);
        });
    },
  }
};
</script>

<style scoped>
  .myAds_container {
    display: flex;
    flex-direction: column;
    padding-bottom: 50px;
    width: 90%;
    margin-right: auto;
    margin-left: auto;
  }
  .myAd {
    display: flex;
    flex-direction: row;
    width: 100%;
    box-shadow: 0px 0px 0px 0.1px black;
    padding: 10px;
  }
  .myAd_imageContainer {
    width: 400px;
    height: 400px;
  }
  .myAd_image {
    width: 40%;
    height: 40%;
    margin-top: auto;
    margin-bottom: auto;
    margin-right: auto;
    margin-left: auto;
  }
  .myAd_main {
    padding: 10px;
  }
  .myAd_main_title {
    font-size: 20px;
    font-weight: 600;
  }
  @media (max-width: 480px) {
    .myAd {
      flex-direction: column;
    }
    .myAd_image {
      width: 100%;
    }
  }

</style>

