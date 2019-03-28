<template>
  <div class="myAds">
    <h1>Ads</h1>
    <div class="myAds_container" v-for="ad in ads" v-bind:key="ad.no">
      <div class="myAd">
        <!-- <div class="myAd_imageContainer"> -->
          <img class="myAd_image" v-bind:src=ad.img >
        <!-- </div> -->
        <div class="myAd_contents">
          <div>title: {{ ad.title }}</div>
          <div>contents: {{ ad.contents }}</div>
          <!-- created_at: {{ ad.created_at }} <br> -->
          <!-- updated_at: {{ ad.updated_at }}<br> -->
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
      ads: []
    };
  },
  mounted: function() {
    this.getAds();
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
    width: 100%;
  }
  .myAd_imageContainer {
    width: 400px;
    height: 400px;
  }
  .myAd_image {
    width: 40%;
    height: 40%;
    padding-right: 20px;
    margin-top: auto;
    margin-bottom: auto;
  }

</style>

