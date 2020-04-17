<template>
        <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :item="3" :nav="false" :dots="false" :autoplay="true">
                        <div class="product-item" v-for="itemproduct in products" :key="itemproduct.id">
                            <div class="pi-pic">
                                <img :src="itemproduct.galleries[0].photo" alt="" />
                                <ul>
                                    <li @click="saveKeranjang(itemproduct.id,itemproduct.name,itemproduct.price,itemproduct.galleries[0].photo)" class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link :to="'/product/'+itemproduct.id">+ Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{itemproduct.type}}</div>
                                <a href="#">
                                    <h5>{{itemproduct.name}}</h5>
                                </a>
                                <div class="product-price">
                                    {{itemproduct.price}}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class="col-lg-12 mt-5" v-else>
                Data product belum tersedia untuk saat ini.
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
    name:'WomanShayna',
    data(){
        return {
            products:[],
            keranjangUser:[]
        }
    },
    components:{
        carousel
    },
    mounted(){
        axios.get('http://localhost:8000/api/products')
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err))
    },
    methods:{
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

<style scoped>
.product-item{
    margin-right: 25px;
}
.pic-pic img{
    height: 450px;
}
</style>
