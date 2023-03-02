<template>

    <v-app id="inspire">
        <div class="home-back">


        <v-app-bar>
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

            <v-toolbar-title>{{ user.level }}</v-toolbar-title>
            <v-spacer />
            <v-btn @click="logout">logout</v-btn>
        </v-app-bar>

        <v-navigation-drawer v-model="drawer">
            <v-sheet color="grey-lighten-4" class="pa-4">
                <div>{{ user.nama }} ({{ user.level }})</div>
            </v-sheet>

            <v-divider></v-divider>

            <v-list>
                <Link href="/masyarakat" as="div">
                <v-list-item to="masyarakat">
                    <template v-slot:prepend>
                        <v-icon>mdi mdi-home-account</v-icon>
                    </template>

                    <v-list-item-title>Home</v-list-item-title>
                </v-list-item>
                </Link>
                <Link href="/masyarakat/pengaduan" as="div">
                <v-list-item to="masyarakat">
                    <template v-slot:prepend>
                        <v-icon>mdi mdi-bug</v-icon>
                    </template>

                    <v-list-item-title>Pengaduan</v-list-item-title>
                </v-list-item>
                </Link>
            </v-list>
        </v-navigation-drawer>

        <v-main>
            <v-container class="py-8 px-6" fluid>
                <slot></slot>
            </v-container>
        </v-main>
        </div>
    </v-app>

</template>


<script>
import { Link, router } from "@inertiajs/vue3";
export default {
    components: {
        Link,
    },
    data: () => ({
        drawer: null,
    }),
    computed: {
        user() {
            return this.$page.props.auth.user
        }
    },
    methods:{
        logout(){
            router.get('/logout')
        }
    }
}
</script>

<style>
.home-back{
    background-image: url("img/background-admin.jpg");
    background-size: cover;
    background-position: center;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
</style>
