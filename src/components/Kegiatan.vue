
<template>
    <div class="form-container">
      <h2>Form Pendaftaran Kegiatan</h2>
      <table>
        
        <tr>
          <td><label for="activity">Kegiatan:</label></td>
          <td><input type="text" id="activity" v-model="newActivity"></td>
        </tr>
        <tr>
          <td><label for="activity">Makanan:</label></td>
          <td><input type="text" id="activity" v-model="newActivity"></td>
        </tr>
        <tr>
          <td><label for="date">Tanggal:</label></td>
          <td><input type="date" id="date" v-model="newDate"></td>
        </tr>
        <tr>
          <td><label for="time">Jam:</label></td>
          <td><input type="time" id="time" v-model="newTime"></td>
        </tr>
        <tr>
          <td colspan="2"><button @click="addActivity">Daftar</button></td>
        </tr>
      </table>
  
      
      <h2>Daftar Kegiatan</h2>
      <table>
        <tr v-for="(activity, index) in activities" :key="index">
          <td>
            <input type="checkbox" v-model="activity.completed" @change="toggleCompletion(activity)">
          </td>
          <td :class="{ 'completed': activity.completed }">{{ activity.name }}</td>
          <td>{{ activity.date }}</td>
          <td>{{ activity.time }}</td>
          <td><button @click="cancelActivity(index)">Batalkan</button></td>
        </tr>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newActivity: '',
        newDate: '',
        newTime:'',
        activities: []
      };
    },
    methods: {
      addActivity() {
        if (this.newActivity && this.newDate) {
          this.activities.push({
            name: this.newActivity,
            date: this.newDate,
            time: this.newTime,
            completed: false
          });
          this.newActivity = '';
          this.newDate = '';
          this.newTime = '';
        }
      },
      cancelActivity(index) {
        this.activities.splice(index, 1);
      },
      toggleCompletion(activity) {
        if (activity.completed) {
          
        }
      }
    }
  };
  </script>
  
  <style scoped>

  input[type="checkbox"] {
    margin-right: 10px;
  }
  
  .completed {
    text-decoration: line-through;
  }
  
  </style>
  