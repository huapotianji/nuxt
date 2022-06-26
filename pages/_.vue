<template>
   <div>一级动态路由</div>
</template>

<script>
export default {
    async asyncData({isDev, route, store, env, params, query, req, res, redirect, error,$axios}) {
        const data =await $axios.$get('/test')
        console.log(data,'接口数据')
    },
    data() {
        return {
            
        }
    },
    mounted(){
        console.log(this.$route,'路由信息')
    }
}
</script>

<style scoped>
</style>