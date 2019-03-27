<template>
  <div class="myAds">
    <h1>Ads</h1>
    <div class="myAds_container" v-for="ad in ads" v-bind:key="ad.no">
      <div class="myAds_imageContainer">
        img: {{ ad.img }}
        <img v-bind:src=ad.img >
      </div>
      <div class="myAds_contents">
        <div>title: {{ ad.title }}</div>
        <div>contents: {{ ad.contents }}</div>
        <!-- created_at: {{ ad.created_at }} <br> -->
        <!-- updated_at: {{ ad.updated_at }}<br> -->
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
</style>

