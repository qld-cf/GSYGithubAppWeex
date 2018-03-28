<template>
    <list class="list" @loadmore="loadMore" @onloadmore="loadMore" loadmoreoffset="30">
        <refresh class="refresh" @refresh="onRefresh"
                 :display="refreshDisplay">
            <text class="indicator-text">Refreshing ...</text>
            <loading-indicator class="indicator"></loading-indicator>
        </refresh>
        <cell :v-if="headerComponent" class="header">
            <component v-bind:is="headerComponent"
                       :itemValue="headerData"></component>
        </cell>
        <cell class="cell" v-for="(rowData, index) in dataList">
            <div class="panel">
                <component v-bind:is="listItemName"
                           :onItemClick="itemClick"
                           :itemValue="rowData"
                           :itemIndex="index"></component>
            </div>
        </cell>
        <cell class="loading">
            <div :style="loadingDisplay">
                <text class="indicator-text">Loading ...</text>
                <loading-indicator class="indicator-loadmore"></loading-indicator>
            </div>
        </cell>
        <cell :style="{height:'200px'}">
            <div :style="{height:'200px'}">
            </div>
        </cell>
    </list>
</template>

<script>
    import * as Constant from '../../core/common/constant'

    export default {
        props: {
            listItemName: {type: String},
            forRefresh: {type: Function},
            forLoadMore: {type: Function},
            itemClick: {type: Function},
            listData: {type: Array},
            headerComponent: {type: String},
            headerData: {type: Object},
        },
        data() {
            return {
                refreshing: false,
                loadinging: false,
                needRefresh: true,
                needLoadMore: true,
            }
        },
        computed: {
            dataList() {
                return this.listData;
            },
            refreshDisplay() {
                let display = (this.refreshing === true && this.needRefresh === true) ? 'show' : 'hide';
                return display
            },
            loadingDisplay() {
                let display = (this.loadinging === true && this.needLoadMore === true) ? {
                    height: '200px',
                    overflow: 'hidden'
                } : {height: '1px', overflow: 'hidden'};
                return display
            }
        },
        methods: {
            loadMore() {
                if (this.refreshing || this.loadinging) {
                    return
                }
                if (this.needLoadMore === false) {
                    return
                }

                this.loadinging = true;
                setTimeout(() => {
                    this.forLoadMore && this.forLoadMore();
                }, 300)
            },
            onRefresh() {
                if (this.refreshing || this.loadinging) {
                    return
                }
                if (this.needRefresh === false) {
                    return
                }
                this.refreshing = true;
                setTimeout(() => {
                    this.forRefresh && this.forRefresh();
                }, 300)
            },
            showRefresh() {
                this.refreshing = true;
            },
            stopLoadMore() {
                this.loadinging = false;
            },
            stopRefresh() {
                this.refreshing = false;
            },
            setNeedLoadMore() {
                this.needLoadMore = true;
            },
            setNeedRefresh() {
                this.needRefresh = true;
            },
            setNotNeedLoadMore() {
                this.needLoadMore = false;
            },
            setNotNeedRefresh() {
                this.needRefresh = false;
            },
        }
    }
</script>

<style scoped>
    .loading {
        margin-top: 20px;
        width: 750px;
        display: -ms-flex;
        display: -webkit-flex;
        display: flex;
        -ms-flex-align: center;
        -webkit-align-items: center;
        -webkit-box-align: center;
        align-items: center;
    }

    .refresh {
        width: 750px;
        height: 200px;
        display: -ms-flex;
        display: -webkit-flex;
        display: flex;
        -ms-flex-align: center;
        -webkit-align-items: center;
        -webkit-box-align: center;
        align-items: center;
    }

    .indicator-text {
        color: #888888;
        font-size: 42px;
        text-align: center;
    }

    .indicator {
        margin-top: 16px;
        height: 40px;
        width: 40px;
        color: blue;
    }

    .indicator-loadmore {
        margin-top: 16px;
        height: 40px;
        width: 40px;
        color: blue;
    }

    .header {
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .panel {
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin-top: 30px;
    }

    .list {
        height: 1334px;
    }
</style>