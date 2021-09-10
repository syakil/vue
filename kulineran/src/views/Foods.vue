<template>
    <div class="foods">
        <Navbar />
        <div class="container">
            <div class="row mt-4">
                <div class="col">
                    <h2>Daftar Menu</h2>
                </div>
                <div class="col-md-3">
                    <div class="input-group mt-2">
                        <input v-model="search" type="text" class="form-control" @keyup="searchProduct" placeholder="Saerch..">
                            <div class="input-group-prepend">
                        <div class="input-group-text"><b-icon-search></b-icon-search></div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row mb-4">
                <div class="col-md-4 mt-4" v-for="product in products" :key="product.id">
                    <Card :product="product"/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from '../components/Navbar.vue'
import Card from '@/components/Card.vue'
import axios from 'axios'

export default {
    name:"Foods",
    components: { Navbar,Card },data() {
    return {
      products:[],
      search:''
    }
    },
    methods:{
        setProduct(data){
        this.products = data
        },
        searchProduct(){
            axios.get('http://localhost:3000/products?q='+this.search)
            .then((response) =>
            // handle success
            this.setProduct(response.data)
            )
            .catch((error)=>
            // handle error
            console.log(error)
            )
        }
    },
    mounted(){
        axios.get('http://localhost:3000/products')
        .then((response) =>
        // handle success
        this.setProduct(response.data)
        )
        .catch((error)=>
        // handle error
        console.log(error)
        )
    }
}
</script>

<style>

</style>