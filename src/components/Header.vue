<template>
    <header>
      <nav>
        <ul>
          <li>
            <button @click="setView('posts')">Post</button>
          </li>
          <li>
            <button @click="setView('todos')">Todos</button>
          </li>
        </ul>
      </nav>
    </header>
  </template>
  
  <script>
  export default {
    methods: {
      setView(view) {
        this.$emit('set-view', view);
      }
    }
  }
  </script>
  
  <style scoped>
  nav ul {
    list-style: none;
    display: flex;
    padding: 0;
    margin: 0;
  }
  
  nav li {
    margin-right: 20px;
  }
  
  nav button {
    background-color: blue;
    color: white;
    border: none;
    cursor: pointer;
    font-size: 1em;
    padding: 10px 20px;
    border-radius: 4px;
  }
  
  nav button:hover {
    background-color: #0056b3;
  }
  </style>
  