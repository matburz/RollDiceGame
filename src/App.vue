<template>
    <v-main>
        <Menu />
    </v-main>
</template>

<script>
import Menu from "./components/Menu";

export default {
    name: "App",

    components: {
        Menu
    },

    data: () => ({}),
    methods: {
        close() {}
    }
};
</script>
