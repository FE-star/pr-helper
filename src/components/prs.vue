<style scoped>
textarea {
    width: 100%;
}
</style>

<script setup>
const props = defineProps({
    data: Object,
    nameMap: Object,
    startTime: String,
})

function judge(item) {
    if (!props.startTime) return true
    const create = +new Date(item.created_at)
    const start = +new Date(props.startTime)
    return start < create
}
</script>

<template>
    <div class="container-fluid">
        <div class="row" v-for="item in (props.data || []).filter(judge)">
            <div class="col-3 col-lg-3">
                <a :href="item.html_url" target="_blank">{{item.title}}</a>
            </div>
            <div class="col-3 col-lg-3">
                <span>{{props.nameMap[item.user.login] || item.user.login}}</span>
            </div>
            <div class="col-6 col-lg-6">
                <textarea placeholder="请写评语"></textarea>
            </div>
        </div>
    </div>
</template>