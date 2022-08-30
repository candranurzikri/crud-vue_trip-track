<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>DATA CATATAN PERJALANAN</h4>
                        <hr>
                        <router-link :to="{name: 'post.create'}" class="btn btn-md btn-success">TAMBAH POST</router-link>

                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">Id</th>
                                    <th scope="col">Id user</th>
                                    <th scope="col">Tanggal</th>
                                    <th scope="col">Jam</th>
                                    <th scope="col">Lokasi</th>
                                    <th scope="col">Suhu</th>
                                    <th scope="col">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(post, index) in catatan" :key="index">
                                    <td>{{ post.id }}</td>
                                    <td>{{ post.id_user }}</td>
                                    <td>{{ post.tanggal }}</td>
                                    <td>{{ post.jam }}</td>
                                    <td>{{ post.lokasi }}</td>
                                    <td>{{ post.suhu }}</td>
                                    <td class="text-center">
                                        <router-link :to="{name: 'post.edit', params:{id: post.id }}" class="btn btn-sm btn-primary mr-1">EDIT</router-link>
                                        <button class="btn btn-sm btn-danger ml-1">DELETE</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'

export default {

    setup() {

        //reactive state
        let catatan = ref([])

        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get('http://127.0.0.1:8000/api/catatan')
            .then(response => {
            
            //assign state posts with response data
            catatan.value = response.data.data

            }).catch(error => {
                console.log(error.response.data)
            })

        })

         //return
    return {
    catatan
    }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>