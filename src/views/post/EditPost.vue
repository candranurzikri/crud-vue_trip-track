<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>EDIT CATATAN</h4>
                        <hr>

                        <form @submit.prevent="update">
                            <div class="form-group">
                                <label for="tanggal" class="font-weight-bold">Tanggal</label>
                                <input type="date" class="form-control" v-model="post.tanggal" placeholder="Masukkan Judul Post">
                                <!-- validation -->
                                <div v-if="validation.tanggal" class="mt-2 alert alert-danger">
                                    {{ validation.tanggal[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="jam" class="font-weight-bold">Jam</label>
                                <input type="time" class="form-control" v-model="post.jam" placeholder="Masukkan Judul Post">
                                <!-- validation -->
                                <div v-if="validation.jam" class="mt-2 alert alert-danger">
                                    {{ validation.jam[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="lokasi" class="font-weight-bold">Lokasi</label>
                                <input type="text" class="form-control" v-model="post.lokasi" placeholder="Masukkan Judul Post">
                                <!-- validation -->
                                <div v-if="validation.lokasi" class="mt-2 alert alert-danger">
                                    {{ validation.lokasi[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="suhu" class="font-weight-bold">Suhu</label>
                                <input type="text" class="form-control" v-model="post.suhu" placeholder="Masukkan Judul Post">
                                <!-- validation -->
                                <div v-if="validation.suhu" class="mt-2 alert alert-danger">
                                    {{ validation.suhu[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary">SIMPAN</button>
                        </form>                        

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        //state posts
        const post = reactive({
            tanggal: '',
            jam: '',
            lokasi: '',
            suhu: ''
        })

        //state validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        //vue router
        const route = useRoute()

        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get(`http://127.0.0.1:8000/api/catatan/${route.params.id}`)
            .then(response => {
            
            //assign state posts with response data
            post.tanggal    = response.data.data.tanggal  
            post.jam  = response.data.data.jam  
            post.lokasi  = response.data.data.lokasi  
            post.suhu  = response.data.data.suhu  

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        //method update
        function update() {

            let tanggal   = post.tanggal
            let jam = post.jam
            let lokasi = post.lokasi
            let suhu = post.suhu

            axios.put(`http://127.0.0.1:8000/api/catatan/${route.params.id}`, {
                tanggal: tanggal,
                jam: jam,
                lokasi: lokasi,
                suhu: suhu
            }).then(() => {

                //redirect ke post index
                router.push({
                    name: 'post.index'
                })

            }).catch(error => {
                //assign state validation with error 
                validation.value = error.response.data
            })

        }

        //return
        return {
            post,
            validation,
            router,
            update
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>