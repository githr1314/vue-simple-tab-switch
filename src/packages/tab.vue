<template>
    <div class="tab" ref="tab">
        <template v-for="(tab, index) in tabs">
            <div 
                :name="tabsName"
                :ref="'tab' + index"
                class="tab-item"
                :key="index"
                :data-key="index"
                @click="changeIndex(tab, index)">
                {{tab.text}}
            </div>
        </template>
        <div ref="line" class="line" :style="lineStyle"></div>
    </div>
</template>

<script>
    export default {
        name: "vue-simple-tab-switch",
        props: {
            tabs: {
                type: Array,
                required: true
            },
            textActiveColor: {
                type: String,
                default: 'rgba(255,85,0,1)'
            },
            textColor: {
                type: String,
                default: 'gray'
            },
            lineColor: {
                type: String,
                default: 'rgba(255,85,0,1)'
            },
        },
        data() {
            return {
                pageIndex: 0,
                timer: '',
                tabsName: 'tabs' + new Date().getTime()
            }
        },
        computed: {
            lineStyle : function() {
                return `background:${this.lineColor}`;
            }
        },
        watch: {
            '$props.tabs': function(val, oldval) {
                if(val.length != oldval.length) {
                    this.selectFirst()
                }
            },
            
            pageIndex : function(val, oldval) {
                let tabWidth;
                let tabItemLeft;
                let tabItemWidth;
                let tabLeftWidth;
                let endLeft;
                requestAnimationFrame(() => {
                    tabWidth = this.$refs['tab'].offsetWidth                    // tab容器宽度
                    tabItemLeft = this.$refs['tab' + val][0].offsetLeft         // tab的左边距宽度
                    tabItemWidth = this.$refs['tab' + val][0].offsetWidth       // tab-item的宽度
                    tabLeftWidth = this.$refs['tab'].scrollWidth - tabWidth     // tab的可滚动宽度
                    endLeft = tabItemLeft + tabItemWidth/2 - tabWidth/2
                });
                requestAnimationFrame(() => {
                    // 改变选中tab的颜色
                    this.$refs['tab' + val][0].style = 'color:' + this.textActiveColor;
                    // 消除之前选中tab的颜色
                    document.getElementsByName(this.tabsName).forEach(dom => {
                        if(dom.style.color != this.textColor && dom.dataset.key != this.pageIndex) {
                            dom.style.color = this.textColor;
                        }
                    })
                    // 移动下标
                    this.$refs.line.style.transform = 'translateX(' + tabItemLeft + 'px)';
                    this.$refs.line.style.width = tabItemWidth + 'px';
                });

                this.timer = setInterval(() => {
                    let scroll = this.$refs['tab'].scrollLeft
                    if(Math.abs(scroll - endLeft) < 2)  {
                        this.$refs['tab'].scrollLeft = endLeft
                        clearInterval(this.timer)
                    }else if(endLeft < 0 && scroll == 0) {
                        clearInterval(this.timer)
                    }else if(Math.abs(scroll - tabLeftWidth) < 2) {                                                                                
                        clearInterval(this.timer)
                    }
                    let speed = (endLeft - scroll)/4
                    if(Math.abs(speed) < 1) {
                        if(speed > 0) {
                            speed = 1
                        }else {
                            speed = -1
                        }
                    }
                    this.$refs['tab'].scrollLeft = scroll + speed;
                }, 17);
            },
        },
        mounted() {
            this.selectFirst();
        },
        methods: {
            selectFirst : function() {
                this.$nextTick(() => {
                    this.$refs.line.style.transform = 'translateX(0px)';
                    let tabItemWidth = this.$refs['tab0'][0].offsetWidth;
                    this.$refs.line.style.width = tabItemWidth + 'px';
                    document.getElementsByName(this.tabsName).forEach(dom => {
                        dom.style.color = this.textColor;
                        if(dom.dataset.key == this.pageIndex){
                            dom.style.color = this.textActiveColor;
                        }
                        dom.addEventListener('mouseover', e => {
                            dom.style.color = this.textActiveColor;
                        });
                        dom.addEventListener('mouseleave', e => {
                            if(dom.dataset.key == this.pageIndex) {
                                dom.style.color = this.textActiveColor;
                            }else {
                                dom.style.color = this.textColor;
                            }
                        });
                    })
                })
            },

            changeIndex : function(item, index) {
                this.pageIndex = index;
                this.$emit('changeTab', {item: Object.assign({}, item), index: index});
            },

        },
        beforeDestroy() {
            clearInterval(this.timer);
        }
    }
</script>

<style scoped>
.tab {
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: center;
    overflow-y: hidden;
    overflow-x: auto;
    padding: 10px 0;
    background: white;
}

.tab-item {
    position: relative;
    white-space: nowrap;
    padding: 0 10px;
}

.tab-item:hover {
    cursor: pointer;
}

.line {
    position: absolute;
    left: 0;
    bottom: 1px;
    height: 2px;
    transition: all 300ms linear .15s;
}

.tab::-webkit-scrollbar {
    display: none;
}

</style>

