<template>
    <div class="background">
      <img src="https://wallpaperwaifu.com/wp-content/uploads/2023/12/retro-tropical-synthwave-thumb.jpg">
    </div>
    <div class="container mt-3"> 
      <button v-if="!showOnlyUnfinished" class="btn btn-info" @click="toggleFilter" style="margin-bottom: 5px;">Tampilkan Belum Selesai</button>
      <button v-else class="btn btn-warning" @click="toggleFilter">Remove Filter</button>
      <table class="table">
        <thead>
          <tr>
            <th>Kegiatan</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in filteredActivities" :key="item.id">
            <td :class="{ 'text-decoration-line-through': item.status === 'Selesai' }">{{ item.name }}</td>
            <td>{{ item.status }}</td>
            <td>
              <button v-if="item.status === 'Belum Selesai'" class="btn btn-success" @click="markAsDone(item)">Selesai</button>
              <button v-else class="btn btn-danger" @click="cancel(item)" style="background-color: orange;margin-right: 8px;">Batal</button>
              <button class="btn btn-danger" @click="removeActivity(item)"style=" margin-left: 8px;" >Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="mb-3">
        <input v-model="newActivity" class="form-control" placeholder="Add new activity">
        <button class="btn btn-primary mt-2" @click="addActivity">Add Activity</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const activities = ref([
    { id: 1, name: 'Makan Siang', status: 'Belum Selesai' },
    { id: 2, name: 'Kuliah satu minggu', status: 'Selesai' },
    { id: 3, name: 'Baca Buku', status: 'Belum Selesai' },
    { id: 4, name: 'Tidur Tepat Waktu', status: 'Selesai' }
  ]);
  const newActivity = ref('');
  const showOnlyUnfinished = ref(false);
  
  const filteredActivities = computed(() => {
    return showOnlyUnfinished.value ? activities.value.filter(a => a.status === 'Belum Selesai') : activities.value;
  });
  
  function addActivity() {
    if (newActivity.value.trim()) {
      activities.value.push({ id: Date.now(), name: newActivity.value, status: 'Belum Selesai' });
      newActivity.value = '';
    }
  }
  
  function markAsDone(activity) {
    activity.status = 'Selesai';
  }
  
  function cancel(activity) {
    activity.status = 'Belum Selesai';
  }
  
  function removeActivity(activity) {
    activities.value = activities.value.filter(a => a !== activity);
  }
  
  function toggleFilter() {
    showOnlyUnfinished.value = !showOnlyUnfinished.value;
  }
  </script>
  
  <style scoped>
  .background img{
    position: fixed;
    top: 0;
    left: 0;
    min-height: 90%;
    min-width: 1500px;
    width: 100%;
    height: 100%;
    overflow: hidden;
    z-index: -2;
    object-fit: cover;
    -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
    filter: brightness(0.8);
  }
  .container{
    height: 100%;
    margin-left: -50%;
  }
  tr td{
    background-color: transparent;
    backdrop-filter: blur(8px);
    color: wheat;
    font-weight: bold;
  }
  tr th{
    background-color: gray;
    font-weight: bolder;
  }
  .form-control{
    background-color: transparent;
    backdrop-filter: blur(5px);
  }
  </style>