<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Add Friend</h5>
      <form class="row g-3"  @submit.prevent="store">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama</label>
          <input type="Text" class="form-control" id="inputEmail4" v-model="friend.nama" />
          <div class="alert alert-ganger" v-if="validation.nama">
            {{ validation.nama[0] }}
          </div>
        </div>

        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">No Tlp</label>
          <input type="number" class="form-control" id="inputPassword4" v-model="friend.no_tlp" />
          <div class="alert alert-ganger" v-if="validation.notlp">
            {{ validation.notlp[0] }}
          </div>
        </div>

        <div class="col-12">
          <label for="inputAddress" class="form-label">Alamat</label>
          <input
            type="text" class="form-control" id="inputAddress" placeholder="Masukan Alamat" v-model="friend.alamat" />
            <div class="alert alert-ganger" v-if="validation.alamat">
            {{ validation.alamat[0] }}
          </div>
        </div>
        
        <div class="col-12">
          <button type="submit" class="btn btn-primary">Add</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const friend = reactive({
      nama: "",
      notlp: "",
      alamat: "",
    });
    const validation = ref([]);
    const router = useRouter();
    function store() {
      let nama = friend.nama;
      let notlp = friend.notlp;
      let alamat = friend.alamat;
      axios
        .post('http://pia.labirin.co.id/api/friends', {
          nama: nama,
          notlp: notlp,
          alamat: alamat,
        })
        .then(() => {
          router.push({
            name: "Home",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      friend,
      validation,
      router,
      store,
    };
  },
};
</script>
