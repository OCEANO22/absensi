<template>
  <div class="container mt-4">
    <h3>{{ isEdit ? "Edit Absensi" : "Tambah Absensi" }}</h3>
    <form @submit.prevent="submitForm">
      <input v-model="form.nama" placeholder="Nama" class="form-control mb-2" required />
      <input v-model="form.nis" placeholder="NIS" class="form-control mb-2" required />
      <select v-model="form.status" class="form-control mb-2">
        <option>Hadir</option>
        <option>Izin</option>
        <option>Alpa</option>
      </select>
      <button type="submit" class="btn btn-primary">
        {{ isEdit ? "Update" : "Simpan" }}
      </button>
      <button v-if="isEdit" type="button" class="btn btn-secondary ms-2" @click="batalEdit">Batal</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ["editItem"],

  data() {
    return {
      form: {
        nama: "",
        nis: "",
        status: "Hadir",
        tanggal: new Date().toISOString().split("T")[0],
      },
      isEdit: false,
      id: null,
    };
  },

  watch: {
    editItem(newVal) {
      if (newVal) {
        this.isEdit = true;
        this.form = {
          nama: newVal.nama,
          nis: newVal.nis,
          status: newVal.status,
          tanggal: newVal.tanggal,
        };
        this.id = newVal.id;
      }
    },
  },

  methods: {
    async submitForm() {
      const url = this.isEdit
        ? `https://backend-absensi.n-oceano22.workers.dev/absensi/${this.id}`
        : `https://backend-absensi.n-oceano22.workers.dev/absensi`;

      const method = this.isEdit ? "PUT" : "POST";

      await fetch(url, {
        method,
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.form),
      });

      this.resetForm();
      this.$emit("refresh-data");
    },

    batalEdit() {
      this.resetForm();
    },

    resetForm() {
      this.form = {
        nama: "",
        nis: "",
        status: "Hadir",
        tanggal: new Date().toISOString().split("T")[0],
      };
      this.id = null;
      this.isEdit = false;
    },
  },
};
</script>
