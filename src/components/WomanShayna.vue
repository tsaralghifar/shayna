<template>
     <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :nav="false" :autoplay="true" :dots="false">
                       
                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
                            <div class="pi-pic">
                                <img v-bind:src="itemProduct.galleries[0].photo" alt="" />
                                <ul>
                                    <li @click="saveKeranjang(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)" class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                                    </li>
                                    <li class="w-icon">
                                        <a href="#"><i class="fa fa-random"></i></a>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <a href="#">
                                    <h5>{{ itemProduct.name }}</h5>
                                </a>
                                <div class="product-price">
                                    ${{ itemProduct.price }}
                                </div>
                            </div>
                        </div>

                    </carousel>
                </div>

                <div class="col-lg-12" v-else>
                    <p>
                        No Data
                    </p>
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
    name: 'WomanShayna',
    components: {
        carousel
    },
    data() {
        return {
            products: [],
            keranjangUser: []
        };
    },
    mounted() {
        axios
        .get("http://127.0.0.1:8000/api/products")
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err));
    },
    methods : { 
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
          var productStored = {
              "id" : idProduct,
              "name" : nameProduct,
              "price" : priceProduct,
              "photo" : photoProduct
          };

          this.keranjangUser.push(productStored);
          const parsed = JSON.stringify(this.keranjangUser);
          localStorage.setItem('keranjangUser', parsed);
      }
    }
};
</script>

<style scoped>
.product-item {
    margin-right: 25px;
}
</style>