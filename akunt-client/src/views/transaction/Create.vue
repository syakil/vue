<template>
  <div class="container my-5">
      <div class="row justify-content-center">
            <div class="col-8">
                <router-link :to="{name:'transaction.index'}"
              class="btn btn-primary btn-sm ronuded shadow mb-3">Back</router-link>
                <div class="card rounded shadow">
                    <div class="card-header">
                        Create Transaction
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="store()">
                            <div class="form-group mb-3">
                                <label for="title">Title</label>
                                <input type="text" class="form-control" id="title" v-model="transaction.title">    
                                <div v-if="validation.title" class="text-danger">
                                    {{validation.title[0]}}
                                </div>
                            </div>
                            <div class="form-group mb-3">
                                <label for="amount">Amount</label>
                                <input type="number" class="form-control" id="amount" v-model="transaction.amount">    
                                <div v-if="validation.amount" class="text-danger">
                                    {{validation.amount[0]}}
                                </div>
                            </div>
                            <div class="form-group mb-3">
                                <label for="time">Time</label>
                                <input type="date" placeholder="yyyy-mm-dd hh:mm:ss" class="form-control" v-model="transaction.time" id="time">    
                                
                            </div>
                            <div class="form-group mb-3">
                                <label for="type">Type</label>
                                <select name="" class="form-select" v-model="transaction.type">
                                    <option value="expense">Expense</option> 
                                    <option value="revenue">Revenue</option>   
                                </select>   
                                <div v-if="validation.title" class="text-danger">
                                    {{validation.type[0]}}
                                </div>
                            </div>
                            <button class="btn btn-outline-primary">Submit</button>
                        </form>
                    </div>
                </div>
            </div>
      </div>
  </div>
</template>

<script>
import {reactive,ref} from 'vue'
import {useRouter} from 'vue-router'
import axios from 'axios'

export default {
    setup(){
        // data binding
        const transaction = reactive({
            title:'',
            ammount:'',
            time:'',
            type:'',
        })

        const validation = ref([]);

        const router = useRouter();

        function store(){
            axios.post(
                'http://127.0.0.1:8000/api/transaction',
                transaction
            )
            .then(()=>
                router.push({
                    name:'transaction.index'
                })
            )
            .catch((err)=>{
                validation.value = err.response.data
            })
        }
        return {
            transaction,
            validation,
            router,
            store
        }
    }
}
</script>

<style>

</style>