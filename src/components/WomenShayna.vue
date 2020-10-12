<template>
    <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :nav="false" :autoplay="true" :dots="false">
                        <div class="product-item" v-for="product in products" :key="product.id">
                            <div class="pi-pic">
                                <img class="photo-cover" v-bind:src="product.galleries[0].photo" alt="" />
                                <ul>
                                    <li @click="saveKeranjang(product.id, product.name, product.price, product.galleries[0].photo)" class="w-icon active">
                                        <a href="#">
                                            <i class="icon_bag_alt"></i>
                                        </a>
                                    </li>
                                    <li class="quick-view"><router-link v-bind:to="'/product/' + product.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ product.type }}</div>
                                <a href="#">
                                    <h5>{{ product.name }}</h5>
                                </a>
                                <div class="product-price">
                                    ${{ product.price }}
                                    <span>${{ product.price + (product.price * 0.10) }}</span>
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class="col-lg-12 mt-5" v-else>
                    <p>Produk Belum Tersedia untuk saat ini</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name:"WomenShayna",
    components: {
        carousel,
    },
    data() {
        return {
            products: [],
            keranjangUser: [],
        };
    },
    methods: {
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

          var productStored = {
              "id": idProduct,
              "name": nameProduct,
              "price": priceProduct,
              "photo": photoProduct,
          }

          this.keranjangUser.push(productStored);
          const parsed = JSON.stringify(this.keranjangUser);
          localStorage.setItem('keranjangUser', parsed);

          window.location.reload();
      }
    },
    mounted() {
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('keranjangUser');
      }
    }
    axios
        .get("http://shayna-backend.belajarkoding.com/api/products")
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err));
    }
};
</script>

<style scoped>
    .product-item {
        margin-right: 20px;
    }

    .photo-cover {
        object-fit: cover;
        width: 400px;
        height: 500px;
    }
</style>