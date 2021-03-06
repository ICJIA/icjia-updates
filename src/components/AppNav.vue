<template>
  <div>
    <v-app-bar
      app
      clipped-left
      :flat="flat"
      color="white"
      height="90"
      elevate-on-scroll
    >
      <v-app-bar-nav-icon
        @click="toggleDrawer"
        aria-label="Click to toggle menu"
        class="hidden-sm-and-up"
      />
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink ml-2 mr-2 hover hidden-xs-and-down"
          contain
          :src="require('../../public/icjia-logo.png')"
          transition="scale-transition"
          :width="logoWidth()"
          @click="
            $router.push('/').catch(err => {
              $vuetify.goTo(0);
            })
          "
        />
        <div
          class="ml-3 hover"
          :class="{
            smallTitle:
              this.$vuetify.breakpoint.xs || this.$vuetify.breakpoint.xs,
            largeTitle:
              this.$vuetify.breakpoint.md ||
              this.$vuetify.breakpoint.lg ||
              this.$vuetify.breakpoint.xl
          }"
          style="font-weight: 900; text-transform: uppercase"
          @click="
            $router.push('/').catch(err => {
              $vuetify.goTo(0);
            })
          "
        >
          {{ appTitle }}
        </div>
      </div>
      <v-spacer></v-spacer>

      <v-btn
        text
        style="font-weight: 900"
        aria-label="Home"
        class="hidden-sm-and-down"
        to="/"
      >
        <span style="font-size: 12px">Home</span>
      </v-btn>

      <v-btn
        :to="link.path === '/home' ? '/' : `${link.path}`"
        text
        class="hidden-sm-and-down"
        style="font-weight: 900"
        :aria-label="link.attributes.title"
        v-for="link in nav"
        :key="link.attributes.title"
      >
        <span v-if="link.attributes.menuTitle" style="font-size: 12px">{{
          link.attributes.menuTitle
        }}</span>
        <span v-else style="font-size: 12px">{{ link.attributes.title }}</span>
      </v-btn>
      <v-btn class="hidden-sm-and-down" text href="https://icjia.illinois.gov">
        ICJIA <v-icon small right>open_in_new</v-icon>
      </v-btn>
      <v-col class="hidden-sm-and-down">
        <div style="width: 250px" class>
          <Search></Search>
        </div>
      </v-col>
    </v-app-bar>
  </div>
</template>

<script>
import _ from "lodash";
import { EventBus } from "@/event-bus";
export default {
  data() {
    return {
      extended: false,
      flat: false,
      dialog: false,
      nav: []
    };
  },
  created() {
    let nav = [];
    this.siteMeta.forEach(item => {
      if (item.attributes.showInNav) nav.push(item);
    });
    let sortedNav = _.orderBy(nav, ["attributes.menuRank"], ["asc"]);
    //console.log(nav);

    this.nav = sortedNav;
  },
  computed: {
    addAppAttr() {
      if (this.$vuetify.breakpoint.xs || this.$vuetify.breakpoint.xs) {
        return false;
      } else {
        return true;
      }
    },
    appTitle() {
      if (this.$vuetify.breakpoint.xs || this.$vuetify.breakpoint.xs) {
        return this.$myApp.config.siteTitleMobile;
      } else {
        return this.$myApp.config.siteTitle;
      }
    }
  },

  mounted() {
    EventBus.$on("unflattenNav", () => {
      this.flat = false;
    });
    document.addEventListener("DOMContentLoaded", () => {
      let arr = [
        ...document.getElementsByClassName("v-carousel__controls__item")
      ];
      arr.forEach(item => {
        item.setAttribute("aria-label", "navigation button");
      });
    });
  },
  props: {
    siteMeta: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    toggleSearch() {
      this.$router.push("/search").catch(() => {
        this.$vuetify.goTo(0);
      });
    },
    toggleDrawer() {
      EventBus.$emit("toggleDrawer");
    },
    logoWidth() {
      if (this.$vuetify.breakpoint.xs || this.$vuetify.breakpoint.sm) {
        return 45;
      } else {
        return 80;
      }
    }
  }
};
</script>

<style>
.nav-title {
  font-weight: 900;
  font-size: 24px;
}
.nav-title.small {
  font-size: 20px;
}

.v-card.searchCard {
  background: #ccc !important;
}
.smallTitle {
  font-size: 14px !important;
}

.largeTitle {
  font-size: 20px !important;
}
</style>
