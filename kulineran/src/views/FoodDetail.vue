<template>
    <div class="foodDetail">
        <Navbar/>
        <div class="container">
            <div class="row mt-5">
                <div class="col">                  
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item"><router-link to="/">Home</router-link></li>
                            <li class="breadcrumb-item"><router-link to="/foods">Dafta Makanan</router-link></li>
                            <li class="breadcrumb-item active">{{product.nama}}</li>
                        </ol>
                    </nav>
                </div>
            </div>
            <div class="row">
                <div class="col-md-6">
                    <img class="img-fluid shadow" height="250px" :src="'/assets/images/'+product.gambar">
                </div>
                <div class="col-md-6">
                    <h2><strong>{{product.nama}}</strong></h2>
                    <hr>
                    <h4>Harga <strong>Rp {{product.harga}}</strong></h4>
                    <form class="mt-4" v-on:submit.prevent>
                        <div class="form-group">
                            <label for="jumlah_pesanan">Jumlah Pesanan</label>
                            <input type="number" class="form-control" v-model="pesan.jumlah_pesanan">
                        </div>
                        <div class="form-group">
                            <label for="keterangan">Keterangan</label>
                            <textarea v-model="pesan.keterangan" name="keterangan" id="keterangan" class="form-control" placeholder="Contoh: Pedas,Sedang,Tidak Pakai Sayuran"></textarea>
                        </div>
                        <button @click="pemesanan" type="submit" class="btn btn-success" > <b-icon-cart></b-icon-cart> Pesan</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import axios from 'axios'
export default {
    name:"FoodDetail",
    components:{
        Navbar
    },
    data(){
        return {
            product:{},
            pesan:{}
        }
    },
    methods:{
       setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pesanan) {
        this.pesan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang"})
            this.$toast.success("Sukses Masuk Keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
            })
            .catch((err) => console.log(err));
        } else {
            this.$toast.error("Jumlah Pesanan Harus diisi", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
            });
        }
    },
},
    mounted(){
        axios
        .get("http://localhost:3000/products/"+this.$route.params.id)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error))
    }
}
</script>

<style>

</style>