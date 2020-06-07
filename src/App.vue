<template>
  <section id="app" class="hero">
    <b-modal :active.sync="isLogoModalActive">
      <p class="image">
        <img src="./assets/logo-jardin.jpg" />
      </p>
    </b-modal>

    <div class="hero-body">
      <div class="container">
        <nav class="level is-mobile">
          <div class="level-left">
            <div class="level-item">
              <a src="#">infos</a>
            </div>
          </div>
          <div class="level-right">
            <div class="level-item">
              <div class="keywords">
                <span
                  class="keyword"
                  v-for="(filter, index) in filters"
                  :key="index"
                  @click="unFilter(filter)"
                >
                  {{ filter }}
                </span>
              </div>
            </div>
          </div>
        </nav>

        <div class="columns is-multiline">
          <div class="column is-one-third" v-for="(item, index) in items" :key="index">
            <figure class="image">
              <img :src="require(`./assets/images/${item.name}`)" />
            </figure>
            <div class="keywords">
              <span
                class="keyword"
                v-for="(tag, index) in item.tags"
                :key="index"
                @click="filter(tag)"
              >{{ tag }}</span>
            </div>
            <div
              v-if="item.contributors"
              class="contributors"
            >with {{ item.contributors.join(', ') }}</div>
            <div class="location">{{ item.location }}</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import data from "./data.json";

export default {
  name: "App",
  data() {
    return {
      isLogoModalActive: true,
      items: data,
      filters: []
    };
  },
  methods: {
    filter(tag) {
      if (!this.filters.includes(tag)) this.filters.push(tag);
      this.items = data.filter(item => {
        return item.tags.some(tag => this.filters.includes(tag));
      });
    },
    unFilter(tag) {
      if (this.filters.includes(tag))
        this.filters = this.filters.filter(item => item !== tag);
      if (this.filters.length === 0) {
        this.items = data;
      } else {
        this.items = data.filter(item => {
          return item.tags.some(tag => this.filters.includes(tag));
        });
      }
    }
  }
};
</script>

<style lang="scss">
@import "~bulma/sass/utilities/_all";

$link: $black;

@import "~bulma";
@import "~buefy/src/scss/buefy";

@font-face {
  font-family: "RomanD";
  src: url("./assets/fonts/romand.woff");
}

.modal-background {
  background-color: transparent;
}

nav a:hover {
  text-decoration: underline;
}

.keyword:hover {
  text-decoration: underline;
}

#app {
  font-family: RomanD, Helvetica, Arial, sans-serif;
  font-weight: 600;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
