<template>
  <div>
    <h1 class="text-center mt-3">Aplikasi Absensi Siswa</h1>
    <AdminPanel :editItem="itemToEdit" @refresh-data="refreshList" />
    <AbsensiList ref="list" @edit-item="editItem" />
  </div>
</template>

<script>
import AdminPanel from './components/AdminPanel.vue';
import AbsensiList from './components/AbsensiList.vue';

export default {
  components: { AdminPanel, AbsensiList },
  data() {
    return {
      itemToEdit: null, // data yang sedang diedit
    };
  },
  methods: {
    refreshList() {
      this.$refs.list.loadData();
      this.itemToEdit = null; // reset setelah edit/tambah
    },
    editItem(item) {
      this.itemToEdit = item; // kirim ke AdminPanel
    },
  },
};
</script>
