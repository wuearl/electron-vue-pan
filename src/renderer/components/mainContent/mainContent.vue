<template>
    <div class="main-content">
        <div class="content-title">
            <span>全部文件</span>
            <span>已全部加载，共{{totalFileCount}}个</span>
        </div>
        <div class="content">
            <router-view :bigIconDatas="rowDatas" :data="gridData" :columns="gridColumns" :filter-key="searchQuery"></router-view> 
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue';
import vTable from '@/basic/v-table/v-table.vue';
import vbigIconList from '@/components/v-bigIconList/v-bigIconList.vue';
import MainContentController from './mainContentController';
import Database from '../../../controller/database';

export default Vue.extend({
    name: 'mainContent',
    components: {
        vTable,
        vbigIconList
    },
    data() {
        return {
            totalFileCount: Math.floor(Math.random() * 100),
            searchQuery: '',
            gridColumns: ['name', 'size', 'date'],
            gridData: null,
            MainContentController,
            rowDatas: null,
            screenWidth: document.body.offsetWidth,
            db: new Database()
        };
    },
    created() {
        this.$http.get('/static/mock/fileList.json', {}).then((result) => {
            this.gridData = result.data;
            // this.rowDatas = this.MainContentController.computeRows(result.data);
            // this.db.insert(this.rowDatas);
            this.db.read((res) => {
                this.rowDatas = res.rowDatas;
                debugger;
            });
        });
        this.$store.dispatch('someAsyncTask');
    },
    mounted() {
        const that = this;
        window.onresize = function temp() {
            if (that.gridData) {
                that.rowDatas = that.MainContentController.computeRows(that.gridData);
            }
        };
    }
});
</script>

<style lang="scss">
    .main-content {
        height: 100%;
        width: 100%;
        background: rgb(255, 255, 255);
        overflow: auto;

        .content-title {
            width: 100%;
            display: inline-block;
            height: 25px;
            background: white;
            border: 0 solid rgb(211, 211, 211);
            border-bottom-width: 1px;
            position: fixed;
            z-index: 9999;

            span {
                margin-left: 10px;
                line-height: 25px;
            }

            span:last-child {
                font-size: 11px;
                float: right;
                margin-right: 10px;
                color: rgb(153, 153, 153);                
            }
        }

        .content {
            background: rgb(255, 255, 255);
            margin-top: 25px;
        }
    }

</style>
