<template>
    <div>
        <tab-switch 
            :tabs="datas2">
        </tab-switch>
        <div class="mt20"></div>
        <tab-switch
            :tabs="datas" 
            :textActiveColor="activeColor"
            :textColor="color"
            :lineColor="activeColor"
            @changeTab="changeTab">
        </tab-switch>
    </div>
</template>

<script>
import tabSwitch from "./packages/tab.vue";
export default {
    components: {
        tabSwitch
    },
    data() {
        return {
            activeColor: '#409eff',
            color: '#c0c4cc',
            datas: [
                {text: '推荐推荐', value: 1},{text: '军事', value: 1},{text: '历史', value: 1},{text: '教育', value: 1},
                {text: '推荐', value: 1},{text: '军事军事', value: 1},{text: '历史', value: 1},{text: '教育', value: 1},
                {text: '推荐', value: 1},{text: '军事', value: 1},{text: '历史历史', value: 1},{text: '教育', value: 1},
                {text: '推荐', value: 1},{text: '军事', value: 1},{text: '历史', value: 1},{text: '教育', value: 1},
            ],
            datas2: [
                {text: '新闻', value: 1},{text: '教育', value: 1},{text: '历史', value: 1},{text: '科学', value: 1}
            ]
        };
    },
    mounted(){
        
    },
    methods: {
        changeTab : function(res) {
            console.log(res);
        }
    }
};
</script>
<style scoped>
.mt20 {
    margin-top: 20px;
}
</style>
