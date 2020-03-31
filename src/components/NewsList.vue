<template>
  <div>
    <v-container fluid style="margin-top: -55px;" class="mb-12">
      <div v-for="category in $myApp.categories" :key="category">
        <h2 :id="slugify(category)">{{ category }}</h2>
        <ul>
          <div
            v-for="item in $myApp.news"
            :key="item.attributes.title"
            class="px-5"
          >
            <li
              v-if="
                item.attributes.category === category && !item.attributes.file
              "
              class="news-list"
            >
              <router-link :to="item.path"
                >{{ item.attributes.posted | dateFormat }} -
                {{ item.attributes.title }}</router-link
              >
              <ul class="summary" v-if="item.attributes.displaySummaryOnHome">
                <li>{{ item.attributes.summary }}</li>
              </ul>
            </li>
            <li
              v-if="
                item.attributes.category === category && item.attributes.file
              "
              class="news-list"
            >
              <a
                :href="
                  `${$myApp.computedPublicPath}/downloads/${item.attributes.file}`
                "
                >{{ item.attributes.posted | dateFormat }} -
                {{ item.attributes.title }}</a
              >
              <ul class="summary" v-if="item.attributes.displaySummaryOnHome">
                <li>{{ item.attributes.summary }}</li>
              </ul>
            </li>
          </div>
        </ul>
      </div>
    </v-container>
  </div>
</template>

<script>
// import _ from "lodash";
import slugs from "slugs";
export default {
  data() {
    return {
      newsItems: null,
      loading: true,
      slugs
    };
  },
  methods: {
    slugify(str) {
      return slugs("icjia-" + str);
    }
  },
  mounted() {
    // let newsItems = this.$myApp.siteMeta.filter(item => {
    //   if (item.root === "/news") {
    //     return item;
    //   }
    // });
    // this.newsItems = newsItems;

    this.loading = false;
  }
};
</script>

<style>
ul.summary {
  list-style-type: none;
  margin-left: -10px;
  margin-top: 5px !important;
}

li.news-list {
  margin-bottom: 8px;
}
</style>
