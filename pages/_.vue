<template>
    <div>
        内容
        <mavon-editor
          :autofocus="false"
          ref="md"
          v-model="formData.mdContent"
          @change="getMdHtml"
          @imgAdd="uploadContentImg"
          @imgDel="delContentImg"
        />
        <span>

        </span>
    </div>
</template>
<script>
import { GaList } from "./data";
export default {
    async asyncData({ isDev, route, store, env, params, query, req, res, redirect, error, $axios }) {
        // const data =await $axios.$get('/test')
        // console.log(data,'接口数据')
        const gaId = GaList.filter(item => req.headers.host.includes(item.Domain))[0].GACode
        env.GOOGLE_ANALYTICS_ID = gaId
    },
    data() {
        return {
            currentComponents: { type: 'view-page-test', key: 'pageTest', params: {}, apiUrl: '/channel_site/' },
            formData:{
                mdContent:''
            }
        }
    },
    mounted() {
        console.log(this.$route, '路由信息')
    },
    methods:{
        getMdHtml(){
            
        },
        uploadContentImg(){

        },
        delContentImg(){

        }
    }
}
</script>

<style scoped>
</style>