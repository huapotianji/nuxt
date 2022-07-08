<template>
        <div v-if="currentComponents && currentComponents.type">
            <component
            :is="currentComponents.type"
            :key="currentComponents.key"
            :params="currentComponents.params"
            :pageData="pageData"
          />
        </div>
</template>
<script>
export default {
    props:{
        pageData:{
            type:Object,
            default:()=>{}
        },
        currentComponents:{
            type:Object,
            default:()=>{}
        }
    }
    
}
</script>
<style lang="">
    
</style>