<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>TAMBAH CATATAN PERJALANAN</h4>
                        <hr>

                        <form @submit.prevent="store">
                            <div class="form-group">
                                <label for="id_user" class="font-weight-bold">id_user</label>
                                <input type="text" class="form-control" v-model="post.id_user" placeholder="Masukkan id_user ">
                                <!-- validation -->
                                <div v-if="validation.id_user" class="mt-2 alert alert-danger">
                                    {{ validation.id_user[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="tanggal" class="font-weight-bold">tanggal</label>
                                <input type="date" class="form-control" v-model="post.tanggal" placeholder="Masukkan tanggal ">
                                <!-- validation -->
                                <div v-if="validation.tanggal" class="mt-2 alert alert-danger">
                                    {{ validation.tanggal[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="Jam" class="font-weight-bold">Jam</label>
                                <input type="time" class="form-control" v-model="post.jam" placeholder="Masukkan jam">
                                <!-- validation -->
                                <div v-if="validation.jam" class="mt-2 alert alert-danger">
                                    {{ validation.jam[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="lokasi" class="font-weight-bold">lokasi</label>
                                <input type="text" class="form-control" v-model="post.lokasi" placeholder="Masukkan lokasi ">
                                <!-- validation -->
                                <div v-if="validation.lokasi" class="mt-2 alert alert-danger">
                                    {{ validation.lokasi[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="suhu" class="font-weight-bold">suhu</label>
                                <input type="text" class="form-control" v-model="post.suhu" placeholder="Masukkan suhu">
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
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        //state posts
        const post = reactive({
            id_user: '',
            tanggal: '',
            jam: '',
            lokasi: '',
            suhu: ''
        })

        //state validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        //method store
        function store() {

            let id_user   = post.id_user
            let tanggal = post.tanggal
            let jam = post.jam
            let lokasi = post.lokasi
            let suhu = post.suhu

            axios.post('http://127.0.0.1:8000/api/catatan', {
                id_user: id_user,
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
            store
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>