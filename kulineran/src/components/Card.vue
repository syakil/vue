<template>
    <div class="card h-100 shadow best-foods">
        <img class="card-img-top" height="250px" :src="'/assets/images/'+product.gambar">
        <div class="card-body">
            <h5 class="card-title">{{product.nama}}</h5>
            <p class="card-text">Harga Rp.{{product.harga}}</p>
            <router-link class="btn btn-success" :to="'/foods/'+product.id">
            Pesan <b-icon-cart></b-icon-cart>
            </router-link>
        </div>
    </div>
</template>

<script>
export default {
    name:"Card",
    props:['product']
}
</script>

<style>

</style>