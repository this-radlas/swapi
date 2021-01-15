<template>
  <div id="app">
    <img src="./assets/logo.png" alt="Star Wars" />
    <ul class="list">
      <li class="item" v-for="(user, index) in users" :key="index">
        <div class="name" @click.self="getDataUser(index)">{{ user.name }}</div>
      </li>
    </ul>

    <transition name="modal">
      <div v-if="showDetail && user">
        <div class="overlay" @click.self="showDetail = false">
          <div class="modal">
            <h2>{{ user.name }}</h2>
            Height: {{ user.height }} <br />
            Mass: {{ user.mass }} <br />
            Hair color: {{ user.hair_color }} <br />
            Skin color: {{ user.skin_color }} <br />
            Eye Color: {{ user.eye_color }} <br />
            Birth year: {{ user.birth_year }} <br />
            Gender: {{ user.gender }} <br /><br />
            <a @click="showDetail = false" href="#">Zavřít</a>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      showDetail: false,
      users: null,
      user: null,
    };
  },
  methods: {
    getData() {
      fetch("https://swapi.dev/api/people/")
        .then((response) => response.json())
        .then((data) => {
          this.users = data.results;
        })
        .catch((err) => console.error(err));
    },
    getDataUser(index) {
      this.user = this.users[index];
      this.showDetail = true;
    },
  },
  created: function () {
    this.getData();
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Oswald&display=swap");

body {
  display: flex;
  justify-content: center;
  background: url("./assets/bg.webp");
  padding: 5em 0;
  font-family: "Oswald", sans-serif;
  font-size: 1rem;
  line-height: 1.6;
}

a {
  background: #000;
}

.list {
  list-style: none;
  padding: 0;
  .item {
    max-width: 300px;
    background: #000;
    padding: 0.5rem 1rem;
    margin-bottom: 1.5rem;
  }
  .name {
    font-size: 2rem;
    color: #fff;
    text-align: center;

    &:hover {
      color: #ffe81f;
      cursor: pointer;
    }
  }
}

.modal {
  max-width: 500px;
  width: 500px;
  min-height: 300px;
  margin: 0px auto;
  padding: 1.5em 3em;
  background-color: #fff;
  box-shadow: 0 2px 40px 10px;
  transition: all 0.2s ease-in;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: #060606cf;
  z-index: 999;
  transition: opacity 0.2s ease;
}
</style>
