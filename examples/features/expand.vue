<template>
<div>
    <h3>展开功能</h3>
    <p>表格行展开 <a href="https://github.com/tm-fe/FlexTable/blob/master/examples/features/expand.vue">source code</a></p>
    <a href="javascript:;" @click="refresh">刷新数据</a>
    <flex-table
        resizable
        :loading="loading"
        :columns="columns"
        :data="list"
        :sum="sum"></flex-table>

    <p>expand scoped slot 用法</p>
    <flex-table
        resizable
        :loading="loading"
        :columns="columns2"
        :data="list2"
        ref="expandTable"
        @on-toggle-expand="onToggleExpand"
        :sum="sum">
        <template slot-scope="{ row, index }" slot="expand">
            <div style="padding: 15px 20px;">
                <div>第 {{ index+1 }} 行</div>
                <!-- <ul>
                    <li>姓名： {{ row.name }}</li>
                    <li>年龄： {{ row.age }}</li>
                    <li>详细地址： {{ row.address }}</li>
                </ul> -->
                <flex-table
                    :columns="columnsEx"
                    :data="list"
                ></flex-table>
            </div>
        </template>
    </flex-table>
</div>
</template>
<script>
// import flexTable from '../../index.js';
import expandRow from './expandRow.vue';

const getMockList = (num = 10) => {
    const aTestList = [];
    for(let i=0;i<num;i++){
        const oTestData = {
            name: 'John Brown',
            age: 18,
            address: 'New York No. 1 Lake Park',
            date: '2016-10-03',
        };
        if (i === 0){
            oTestData.expandStatus = true;
        }
        aTestList.push(oTestData);
    }
    return aTestList;
}

export default {
    // components:{
    //     flexTable
    // },
    data(){
        return {
            columns: [
                {
                    type: 'expand',
                    width: 50,
                    render: (h, params) => {
                        return h(expandRow, {
                            props: {
                                row: params.row
                            }
                        })
                    }
                },
                {
                    title: 'Name',
                    key: 'name',
                },
                {
                    title: 'Age',
                    key: 'age',
                    render(h, params){
                        return h('span', 'age: '+ params.row.age)
                    }
                },
                {
                    title: 'Address',
                    key: 'address',
                },
                {
                    title: 'Date',
                    key: 'date',
                },
            ],
            columns2: [
                {
                    type: 'expand',
                    width: 50
                },
                {
                    title: 'Name',
                    key: 'name'
                },
                {
                    title: 'Age',
                    key: 'age',
                    render(h, params){
                        return h('span', 'age: '+ params.row.age)
                    }
                },
                {
                    title: 'Address',
                    key: 'address',
                },
                {
                    title: 'Date',
                    key: 'date',
                },
            ],
            columnsEx: [
                {
                    title: 'Name',
                    key: 'name',
                },
                {
                    title: 'Address',
                    key: 'address',
                },
                {
                    title: 'Date',
                    key: 'date',
                }
            ],
            loading: false,
            list: getMockList(),
            list2: getMockList(),
            sum:{
                name: 'Jim Green',
                age: 24,
                address: 'London',
                date: '2016-10-01',
            },
        }
    },
    mounted() {},
    methods: {
        toggle(index){
            this.$refs.expandTable.toToggleExpand(index);
        },
        onToggleExpand(index, status) {
            const obj = Object.assign({}, this.list2[index]);
            obj.expandStatus = status;
            this.$set(this.list2, index, obj);
        },
        refresh() {
            const num = parseInt(Math.random()*(10+1),10);
            console.log(num);
            this.list = getMockList(num);
        }
    }
}
</script>