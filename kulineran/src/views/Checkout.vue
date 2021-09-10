<template>
    <div class="checkout">
        <Navbar/>
        <div class="container">
            <div class="row justify-content-center mt-5">
                <div class="col text-center">
                    <img src="../assets/images/menunggu.png" width="300" />
                    <h2><strong>Sukses!!</strong></h2>
                    <h4>Pesanan Anda segera diproses <br> Selamat Menunggu</h4>
                    <br>
                    <router-link to="/foods" class="btn btn-success"> <b-icon-cart></b-icon-cart> Pesan Lagi</router-link>
                    <br>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
export default {
    name: "Checkout",
    components:{
        Navbar
    }
}
</script>

<style>

</style>