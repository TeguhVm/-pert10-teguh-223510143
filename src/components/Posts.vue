<template>
    <div class="posts-container">
      <h1>POSTS</h1>
      <label for="userSelect">Select User:</label>
      <select v-model="selectedUserId" @change="fetchPosts">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <ul v-if="posts.length > 0" class="posts-list">
        <li v-for="post in posts" :key="post.id" class="post-item">
          <h2>{{ post.title }}</h2>
          <p>{{ post.body }}</p>
        </li>
      </ul>
      <p v-else-if="loading" class="loading">Loading...</p>
      <p v-else class="no-posts">Tidak ada postingan.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        posts: [],
        selectedUserId: null,
        loading: false,
        postsCache: {}
      };
    },
    created() {
      this.fetchUsers();
    },
    methods: {
      fetchUsers() {
        this.loading = true;
        fetch('https://jsonplaceholder.typicode.com/users')
          .then(response => response.json())
          .then(data => {
            this.users = data;
            this.loading = false;
          });
      },
      fetchPosts() {
        if (this.selectedUserId) {
          if (!this.postsCache[this.selectedUserId]) {
            this.loading = true;
            fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
              .then(response => response.json())
              .then(data => {
                this.postsCache[this.selectedUserId] = data;
                this.posts = data;
                this.loading = false;
              });
          } else {
            this.posts = this.postsCache[this.selectedUserId];
          }
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .posts-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  label {
    display: block;
    margin-bottom: 10px;
    font-weight: bold;
  }
  
  select {
    width: 100%;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ddd;
    margin-bottom: 20px;
    transition: border-color 0.3s;
  }
  
  select:focus {
    border-color: #888;
  }
  
  .posts-list {
    list-style: none;
    padding: 0;
  }
  
  .post-item {
    background: #fff;
    padding: 20px;
    margin-bottom: 15px;
    border-radius: 5px;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s, box-shadow 0.2s;
  }
  
  .post-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 3px 15px rgba(0, 0, 0, 0.1);
  }
  
  .post-item h2 {
    margin: 0 0 10px;
    color: #333;
    font-size: 1.5em;
  }
  
  .post-item p {
    margin: 0;
    color: #555;
    line-height: 1.6;
  }
  
  .loading, .no-posts {
    text-align: center;
    font-size: 1.2em;
    color: #666;
  }
  </style>
  