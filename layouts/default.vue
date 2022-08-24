<template>
    <v-app>
        <v-navigation-drawer
            v-model="drawer"
            :mini-variant="miniVariant"
            :clipped="clipped"
            fixed
            app
        >
            <v-list>
                <v-list-item
                    v-for="(item, i) in items"
                    :key="i"
                    :to="item.to"
                    router
                    exact
                >
                    <v-list-item-action>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title v-text="item.title" />
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-app-bar :clipped-left="clipped" fixed app>
            <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

            <v-toolbar-title v-text="title" />
            <v-spacer />
            <div v-if="$auth.loggedIn">
            <v-menu
            bottom
            left
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
                        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              {{ $auth.user.name }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ $auth.user.email }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
              <v-list-item @click="logout">
          <v-list-item-icon>
            <v-icon>mdi-logout</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title >Logout</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
            </v-list>
          </v-menu>
            </div>
        </v-app-bar>

        <v-main>
            <v-container>
                <nuxt keep-alive />
            </v-container>
        </v-main>
        <v-footer :absolute="!fixed" app>
            <span>&copy; {{ new Date().getFullYear() }}</span>
        </v-footer>
    </v-app>
</template>

<script>
export default {
    name: "DefaultLayout",
    data() {
        return {
            clipped: false,
            drawer: false,
            fixed: false,
            items: [
                {
                    icon: "mdi-apps",
                    title: "Welcome",
                    to: "/",
                },
                {
                    icon: "mdi-chart-bubble",
                    title: "Inspire",
                    to: "/inspire",
                },
                {
                    icon: "mdi-chart-bubble",
                    title: "Products",
                    to: "/products",
                },
            ],
            miniVariant: false,
            right: true,
            rightDrawer: false,
            title: "Vuetify.js",
        };
    },
    methods: {
        async logout() {
            await this.$auth.logout();
        },
    },
};
</script>
