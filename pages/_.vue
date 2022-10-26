<template lang="md">
  # Hello World!

  Current route is: {{ $route.path }}
</template>

<script>
export default {
    async asyncData({ isDev, route, store, env, params, query, req, res, redirect, error, $axios }) {
        // const data =await $axios.$get('/test')
        // console.log(data,'接口数据')
    },
    data() {
        return {
            currentComponents: { type: 'view-page-test', key: 'pageTest', params: {}, apiUrl: '/channel_site/' },
        }
    },
    mounted() {
        console.log(this.$route, '路由信息')
    }
}
</script>

<style scoped>
</style>