<template>
    <div class="keranjang">
        <Navbar :updateKeranjang="keranjangs"/>
        <div class="container">
            <div class="row mt-5">
                <div class="col">                  
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item"><router-link to="/">Home</router-link></li>
                            <li class="breadcrumb-item"><router-link to="/foods">Dafta Makanan</router-link></li>
                            <li class="breadcrumb-item active">Keranjang</li>
                        </ol>
                    </nav>
                </div>
            </div>
            <div class="row">
                <h2>Keranjang</h2>
            </div>
            <div class="row">
                <div class="col-md-8">
                    <div class="table-responsive mt-4">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Foto</th>
                                    <th scope="col">Makanan</th>
                                    <th scope="col">Keterangan</th>
                                    <th scope="col">Jumlah</th>
                                    <th scope="col">Harga</th>
                                    <th scope="col">Sub Total</th>
                                    <th scope="col">Aksi</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(keranjang,index) in keranjangs" :key="keranjang.id">
                                    <th scope="row">{{index+1}}</th>
                                    <td><img :src="'/assets/images/'+ keranjang.products.gambar" class="img-fluid shadow" width="90px" alt=""></td>
                                    <td>{{keranjang.products.nama}}</td>
                                    <td>{{keranjang.keterangan ? keranjang.keterangan : "-"}}</td>
                                    <td>{{keranjang.jumlah_pesanan}}</td>
                                    <td>{{keranjang.products.harga}}</td>
                                    <td>{{keranjang.products.harga * keranjang.jumlah_pesanan}}</td>
                                    <td>
                                        <button @click="hapusKeranjang(keranjang.id)" class="btn btn-danger">
                                            <b-icon-trash></b-icon-trash>
                                        </button>
                                    </td>
                                </tr>
                                <tr>
                                    <td colspan="6" align="right">
                                        <strong>Total Harga :</strong>
                                    </td>
                                    <td>
                                        <strong>{{totalHarga}}</strong>
                                    </td>
                                    <td></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                <div class="col-md-4">
                    <form class="mt-4" v-on:submit.prevent>
                        <div class="form-group">
                        <label for="nama">Nama :</label>
                        <input type="text" class="form-control" v-model="pesan.nama" />
                        </div>
                        <div class="form-group">
                        <label for="noMeja">Nomor Meja :</label>
                        <input type="text" class="form-control" v-model="pesan.noMeja" />
                        </div>

                        <button type="submit" class="btn btn-success float-right" @click="checkout">
                        <b-icon-cart></b-icon-cart>Pesan
                        </button>
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
    name:"Keranjang",
    components:{
        Navbar
    },
    data(){
        return{
            keranjangs:[],
            pesan:{}
        }
    },
    methods:{
        
        dataKeranjang(){
            axios.get('http://localhost:3000/keranjangs')
            .then((response) => this.setKeranjang(response.data))
            .catch((error)=>console.log(error))
        },
        setKeranjang(data){
            this.keranjangs = data
        },
        hapusKeranjang(id){    
            axios.delete('http://localhost:3000/keranjangs/'+id)
            .then(() =>
                this.dataKeranjang(),
                this.$toast.success("Sukses Hapus Keranjang", {
                type: "success",
                position: "top-right",
                duration: 3000,
                dismissible: true,
                })
            )
            .catch((error)=>console.log(error))
        },
        checkout(){
            if (this.pesan.nama && this.pesan.noMeja) {
                
                this.pesan.keranjangs = this.keranjangs

                axios.post("http://localhost:3000/pesanans", this.pesan)
                .then(() => {
                    this.$router.push({ path: "/checkout"})
                    this.$toast.success("Sukses Chekout Keranjang", {
                    type: "success",
                    position: "top-right",
                    duration: 3000,
                    dismissible: true,
                    });
                })

                this.keranjangs.map(function(item){
                    return axios.delete('http://localhost:3000/keranjangs/'+ item.id)
                    .catch((error)=>console.log(error))
                })

            }else{
                this.$toast.error("Nama Dan No Meja Harus Diisi!", {
                type: "error",
                position: "top-right",
                duration: 3000,
                dismissible: true,
                })
            }
        }
    },
    mounted(){
        this.dataKeranjang()
    },
    computed:{
        totalHarga(){
            return this.keranjangs.reduce(function(item,data){
                return item + (data.products.harga * data.jumlah_pesanan)
            },0)
        }
    }
}
</script>

<style>

</style>