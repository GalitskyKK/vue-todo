<template>
  <div id="app">
    <header>
      <div class="logo">KK</div>
      <h1>Vue To-Do</h1>
    </header>
    <main>
      <TodoList />
    </main>
    <footer>
      <p>&copy; KK 2024</p>
    </footer>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoList,
  },
};
</script>

<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  background-color: #ffffff;
  color: #000000;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

#app {
  width: 100%;
  max-width: 800px;
  padding: 20px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40px;
}

header .logo {
  font-size: 36px;
  font-weight: bold;
  letter-spacing: 2px;
  color: #000000;
}

h1 {
  font-size: 24px;
  font-weight: normal;
  margin: 0;
}

main {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 1px 5px rgba(0, 0, 0, 0.1);
  color: #000000;
}

footer {
  text-align: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid #4444445d;
  color: #777;
}

footer p {
  margin: 0;
}
</style>
