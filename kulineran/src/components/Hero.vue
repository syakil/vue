<template>
    <div class="hero">
        <div class="row">
            <div class="col-md-6">
                <img src="../assets/images/hero.png" width="100%">
            </div>
            <div class="col-md-6">
                <div class="d-flex h-100">
                    <div class="justify-content-center align-self-center">
                        <h2><strong>Delicous Food Menu,</strong><br> In Your Gadget</h2>
                        <p>Ayo Segera Pilih Makanan Kesukaan Mu !</p>
                        <router-link to="/foods" class="btn btn-success"><b-icon-cart></b-icon-cart>&ensp; Pesan</router-link>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:"Hero"
}
</script>

<style>

</style>