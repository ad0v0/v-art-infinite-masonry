<template>
  <section>
    <h1 class="heading">V-Art Gallery</h1>

    <div class="gallery-wrapper">
      <vue-masonry-wall
        :items="items"
        :options="{width: 300, padding: 12}"
        @append="append"
      >
        <template v-slot:default="{item}">
          <div class="item">
            <img :src="item.download_url" alt="Art" />

            <div class="content">
              <p>№: {{ item.indexNumber }}</p>
              <p>Author: {{ item.author }}</p>
              <p>ID: {{ item.id }}</p>
            </div>
          </div>
        </template>
      </vue-masonry-wall>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import VueMasonryWall from 'vue-masonry-wall';

export default {
  name: 'app',
  components: {
    VueMasonryWall
  },
  data() {
    return {
      items: []
    }
  },
  methods: {
    append: function () {
      this.getData();
      this.getIndex();
    },
    getData: function () {
      axios
        .get("https://picsum.photos/v2/list?page=1&limit=10")
        .then(res => (
          this.items = this.items.concat(res.data)
        ));
    },
    getIndex: function () {
      this.items.forEach((item, index) => {
        item.indexNumber = ++index;
      })
    }
  }
}
</script>

<style scoped>

.heading {
  text-align: center;
}

.gallery-wrapper {
  max-width: 600px;
  margin: 0 auto;
}

.item {
  overflow: hidden;
  border-radius: 4px;
  width: 100%;

  background: #F5F5F5;
}

.content {
  padding: 20px;
}

img {
  object-fit: cover;
  width: 100%;
  height: 100%;
  line-height: 0;
  display: block;
}

</style>
