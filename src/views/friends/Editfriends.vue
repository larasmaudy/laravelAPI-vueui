<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Add Friend</h5>
      <form class="row g-3">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama</label>
          <input type="Text" class="form-control" id="inputEmail4" />
        </div>
        
        <div class="col-md-6">
          <label for="inputPassword4" class="form-label">No Tlp</label>
          <input type="number" class="form-control" id="inputPassword4" />
        </div>

        <div class="col-12">
          <label for="inputAddress" class="form-label">Alamat</label>
          <input
            type="text"
            class="form-control"
            id="inputAddress"
            placeholder="Masukan alamat"
          />
        </div>
        
        <div class="col-12">
          <button type="submit" class="btn btn-primary">Add</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const friend = reactive({
      nama: "",
      no_tlp: "",
      alamat: "",
    });

    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(()=>(
      axios.get(`http://pia.labirin.co.id/api/friends/${route.params.id}`)
      .then(response => {
        console.log(response.data.data.nama)

        friend.nama = response.data.data.nama
        friend.no_tlp = response.data.data.no_tlp
        friend.alamat = response.data.data.alamat
      }).catch(error => {
        console.log(error.response.data)
      })
    ))

    function update() {
      let nama = friend.nama;
      let notlp = friend.notlp;
      let alamat = friend.alamat;

      axios.put(`http://pia.labirin.co.id/api/friends/${route.params.id}`, {
          nama: nama,
          notlp: notlp,
          alamat: alamat,
        })
        .then(() => {
          router.push({
            name: 'Home',
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
      update,
      route
    };
  },
};
</script>
