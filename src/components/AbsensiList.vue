<template>
  <div class="container mt-4">
    <h3>Data Absensi</h3>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Nama</th>
          <th>NIS</th>
          <th>Status</th>
          <th>Tanggal</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in siswa" :key="s.id">
          <td>{{ s.nama }}</td>
          <td>{{ s.nis }}</td>
          <td>{{ s.status }}</td>
          <td>{{ s.tanggal }}</td>
          <td>
            <button class="btn btn-warning btn-sm me-1" @click="$emit('edit-item', s)">Edit</button>
            <button class="btn btn-danger btn-sm" @click="hapus(s.id)">Hapus</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      siswa: [],
    };
  },
  methods: {
    async loadData() {
      try {
        const res = await fetch("https://backend-absensi.n-oceano22.workers.dev/absensi");
        this.siswa = await res.json();
      } catch (err) {
        console.error("Gagal mengambil data:", err);
      }
    },
    async hapus(id) {
      if (confirm("Yakin ingin menghapus data ini?")) {
        try {
          await fetch(`https://backend-absensi.n-oceano22.workers.dev/absensi/${id}`, {
            method: "DELETE",
          });
          this.loadData(); // refresh setelah hapus
        } catch (err) {
          console.error("Gagal menghapus:", err);
        }
      }
    },
  },
  created() {
    this.loadData();
  },
};
</script>
