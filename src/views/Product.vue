<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/" href="./home.html"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_preview" alt="" />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                  <carousel class="product-thumbs-track ps-slider" :nav="false" :dots="false">

                    <div 
                      v-for="ss in productDetails.galleries" :key="ss.id"
                      class="pt" @click="changeImage(ss.photo)" 
                      :class="ss.photo == gambar_preview ? 'active' : ''">
                      <img :src="ss.photo" alt="" />
                    </div>

                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{  productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <div v-html="productDetails.description">
                    </div>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart" class="primary-btn pd-cart">
                      <a @click="saveKeranjang(productDetails.id,productDetails.name,productDetails.price,productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cartz">Add To Cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct />

    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "@/components/HeaderShayna.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import carousel from "vue-owl-carousel";

import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    RelatedProduct,
    FooterShayna,
    carousel,
  },
  data() {
    return {
      gambar_preview: "",
      productDetails: [],
      keranjangUser: [],
    }
  },
  methods: {
    changeImage(url) {
      this.gambar_preview = url;
    },
    setDataPicture(data) {
      this.productDetails = data;
      this.gambar_preview = data.galleries[0].photo;
    },
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        'id': idProduct,
        'name': nameProduct,
        'price': priceProduct,
        'photo': photoProduct
      };
      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem('keranjangUser', parsed);
    }
  },
  mounted() {
    axios
      .get("http://128.199.98.129:445/api/products", {
        params: {
          id: this.$route.params.id
        }
      })
      .then(res => (this.setDataPicture(res.data.data)))
      // eslint-disable-next-line no-console 
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>
