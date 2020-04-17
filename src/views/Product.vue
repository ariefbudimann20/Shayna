<template>
  <div class="product">
    <HeaderShayna/>
          <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link >
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
                                <img class="product-big-img" :src="gambar_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider"  :autoplay="true" :dots="false" :nav="false">
                                    <div v-for="item in productDetails.galleries" :key="item.id" class="pt" @click="changeImage(item.photo)" :class="item.photo == gambar_default ?'active' : ''">
                                        <img :src="item.photo" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details">
                                <div class="pd-title text-left">
                                    <span>{{productDetails.type}}</span>
                                    <h3>{{productDetails.name}}</h3>
                                </div>
                                <div class="pd-desc text-left">
                                    <p v-html="productDetails.description"></p>
                                    <h4>$ {{productDetails.price}}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart">
                                         <a href="#"  @click="saveKeranjang(productDetails.id,productDetails.name,productDetails.price,productDetails.galleries[0].photo)" class="primary-btn pd-cart">Add To Cart</a>
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

    <RelatedShayna/>
    <FooterShayna/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import RelatedShayna from '@/components/RelatedShayna.vue'; 
import FooterShayna from '@/components/FooterShayna.vue';
import carousel from 'vue-owl-carousel';
import axios from 'axios';


export default {
  name: 'Product',
  data(){
    return{
        gambar_default:"",
        productDetails: [],
        keranjangUser:[]
    }
  },

    components: {
        HeaderShayna,
        RelatedShayna,
        FooterShayna,
        carousel
    },

    mounted(){
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios.get('http://localhost:8000/api/products',{
            params:{
                id: this.$route.params.id
            }
        })
        .then(res => (this.setDataProduct(res.data.data)))
        .catch(err => console.log(err))
    },

    methods:{
        changeImage(urlImage){
            this.gambar_default = urlImage;
        },

        setDataProduct(data){
            this.productDetails = data
            this.gambar_default = data.galleries[0].photo
        },

        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct){
            var productStored = {
                'id': idProduct,
                'name': nameProduct,
                'price': priceProduct,
                'photo': photoProduct,
            }
            this.keranjangUser.push(productStored);
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem('keranjangUser', parsed);
        }
    }
}
</script>

<style scope>
  .product-thumbs .pt {
    margin-right: 14px;
  }
</style>
