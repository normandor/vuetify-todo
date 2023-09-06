<template>
  <v-app id="inspire">
    <v-navigation-drawer 
      v-model="drawer"
      app
      mobile-breakpoint="768"
    >
    <v-img
      class="pa-4 pt-7"
      src="mountains.jpeg" 
      height="170"
      gradient="to top right, rgba(19,84,122,.9), rgba(128,208,199,.9)"
    >
      <v-avatar size="70" class="mb-2">
        <img
          src="https://cdn.vuetifyjs.com/images/john.jpg"
          alt="John"
        >
      </v-avatar>
      <div class="font-weight-bold text-subtitle-1 white--text">John John</div>
      <div class="text-subtitle-2 white--text">@johnjohn</div>
    </v-img>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          :to="item.to"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="primary"
      dark
      prominent
      :height="$route.path === '/' ? '220px' : '170px'"
      src="mountains.jpeg"
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(19,84,122,.9), rgba(128,208,199,.9)"
        ></v-img>
      </template>

      <v-container class="header-container pa-0">
        <v-row>
          <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
          <v-spacer></v-spacer>
          <search></search>
        </v-row>
        <v-row>
          <v-app-bar-title class="text-h4 ml-4">{{ $store.state.appTitle }}</v-app-bar-title>
        </v-row>
        <v-row>
          <live-date-time />
        </v-row>       
        <v-row v-if="$route.path === '/'">
          <field-add-task />
        </v-row>
      </v-container>
    </v-app-bar>

    <v-main>
      <router-view></router-view>
      <snackbar :snackbar="true" :text="'test snackbar'" />
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({ 
      drawer: null,
      items: [
          { title: 'Todo', icon: 'mdi-format-list-checks', to: '/' },
          { title: 'About', icon: 'mdi-help-box', to: '/about' },
        ],
    }),
    mounted() {
      this.$store.dispatch('getTasks');
    },
    components: {
      'field-add-task': require('@/components/Todo/FieldAddTask.vue').default,
      'live-date-time': require('@/components/Tools/LiveDateTime.vue').default,
      'search': require('@/components/Tools/Search.vue').default,
      'snackbar': require('@/components/Shared/Snackbar.vue').default,
    }
  }
</script>

<style lang="sass">
  .header-container
    max-width: none !important
</style>