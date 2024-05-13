<template>
  <div class="form-container">
    <!-- Tombol untuk memilih template form pendaftaran kegiatan -->
    <button @click="showRegistrationForm = true" :class="{ 'active': showRegistrationForm }">Form Pendaftaran Kegiatan</button>
    <!-- Tombol untuk memilih template daftar kegiatan -->
    <button @click="showRegistrationForm = false" :class="{ 'active': !showRegistrationForm }">Daftar Kegiatan</button>
    <button @click="showPost = true; showTodos = false" :class="{ 'active': showPost }">Post</button>
    <button @click="showTodos = true; showPost = false" :class="{ 'active': showTodos }">Todos</button>

    <!-- Template Form Pendaftaran Kegiatan -->
    <div v-if="showRegistrationForm">
      <h2>Form Pendaftaran Kegiatan</h2>
      <table>
        <tr>
          <td><label for="activity">Kegiatan:</label></td>
          <td><input type="text" id="activity" v-model="newActivity"></td>
        </tr>
        <tr>
          <td><label for="food">Makanan:</label></td>
          <td><input type="text" id="food" v-model="newFood"></td>
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
    </div>

    <!-- Template Daftar Kegiatan -->
    <div v-else>
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

    <!-- Template Todos -->
    <div v-if="showTodos">
      <h2>Todos</h2>
      <!-- Tambahkan tampilan Todos di sini -->
    </div>

    <!-- Template Post -->
    <div v-if="showPost">
      <h2>Post</h2>
      <select v-model="selectedUser">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <div v-if="selectedUser !== null">
        <h3>Postingan oleh {{ users.find(user => user.id === selectedUser).name }}</h3>
        <ul>
          <li v-for="post in userPosts" :key="post.id">{{ post.title }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: '',
      newFood: '',
      newDate: '',
      newTime: '',
      activities: [],
      showRegistrationForm: true,
      showPost: false,
      showTodos: false,
      selectedUser: null,
      users: [],
      userPosts: []
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity && this.newDate) {
        this.activities.push({
          name: this.newActivity,
          food: this.newFood,
          date: this.newDate,
          time: this.newTime,
          completed: false
        });
        this.newActivity = '';
        this.newFood = '';
        this.newDate = '';
        this.newTime = '';
      }
    },
    cancelActivity(index) {
      this.activities.splice(index, 1);
    },
    toggleCompletion(activity) {
      if (activity.completed) {
        // Logika ketika kegiatan ditandai sebagai selesai
      }
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(users => {
          this.users = users;
        });
    },
    fetchUserPosts(userId) {
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
        .then(response => response.json())
        .then(posts => {
          this.userPosts = posts;
        });
    }
  },
  mounted() {
    this.fetchUsers();
  },
  watch: {
    selectedUser(newValue) {
      if (newValue !== null) {
        this.fetchUserPosts(newValue);
      }
    }
  }
};
</script>

<style scoped>
/* CSS untuk tombol aktif */
button.active {
  background-color: #4CAF50; /* Hijau */
  color: white;
}

/* CSS untuk checkbox */
input[type="checkbox"] {
  margin-right: 10px;
}

/* CSS untuk garis melintang pada kegiatan yang selesai */
.completed {
  text-decoration: line-through;
}
</style>
