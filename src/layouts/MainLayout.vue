<template>
    <q-layout view="lHh Lpr lFf">
        <q-header elevated class="header">
            <q-toolbar>
                <q-toolbar-title class="q-ml-lg"> OCR AI </q-toolbar-title>
            </q-toolbar>
        </q-header>

        <q-page-container class="page-container">
            <router-view />
        </q-page-container>
    </q-layout>
</template>

<script>
const linksList = [
    {
        title: 'Docs',
        caption: 'quasar.dev',
        icon: 'school',
        link: 'https://quasar.dev',
    },
    {
        title: 'Github',
        caption: 'github.com/quasarframework',
        icon: 'code',
        link: 'https://github.com/quasarframework',
    },
    {
        title: 'Discord Chat Channel',
        caption: 'chat.quasar.dev',
        icon: 'chat',
        link: 'https://chat.quasar.dev',
    },
    {
        title: 'Forum',
        caption: 'forum.quasar.dev',
        icon: 'record_voice_over',
        link: 'https://forum.quasar.dev',
    },
    {
        title: 'Twitter',
        caption: '@quasarframework',
        icon: 'rss_feed',
        link: 'https://twitter.quasar.dev',
    },
    {
        title: 'Facebook',
        caption: '@QuasarFramework',
        icon: 'public',
        link: 'https://facebook.quasar.dev',
    },
    {
        title: 'Quasar Awesome',
        caption: 'Community Quasar projects',
        icon: 'favorite',
        link: 'https://awesome.quasar.dev',
    },
];

export default {
    name: 'MainLayout',

    data() {
        return {
            linksList,
        };
    },
};
</script>

<style scoped>
.header {
    background: #0671e0;
}

.page-container {
    padding: 80px;
    background-color: #f9f9f9;
}
</style>
