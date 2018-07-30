<template>
    <div>
        <eg-table
            :tableData="tables.tableData"
            :options="tables.options"
            :page="tables.page"
            :feedback="tables.feedback"
            @handle="tables.handle"
            @handlePage="tables.handlePage"
            @handleSort="tables.handleSort"
        />
    </div>
</template>

<script>
    import tables from '@/components/tables';

    export default {
        name: "index",
        components: {
            'eg-table':  tables
        },
        data(){
            return{
                tables: {
                    options: [
                        {prop: 'year', label: '日期'},
                        {prop: 'name', label: '姓名'},
                        {prop: 'rank', label: '顺位', render: [
                                {myTarget: 7, mode: 'less', myClass: 'bbb'},
                                {myTarget: 11, myClass: 'aaa'}
                            ]
                        },
                        {prop: 'age', label: '年龄', sort: 'custom'},
                        {label: '操作', slot: [
                                {type: 'detail', name: '查看'},
                                {type: 'delete', name: '删除'},
                            ], center: true,
                        },
                    ],
                    tableData: [
                        {year: '2003', name: 'LeBron James', rank: 1, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 2, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26},
                        {year: '2003', name: 'LeBron James', rank: 1, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 11, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26},
                        {year: '2003', name: 'LeBron James', rank: 10, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 1, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26},
                        {year: '2003', name: 'LeBron James', rank: 1, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 1, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26},
                        {year: '2003', name: 'LeBron James', rank: 1, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 1, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26},
                        {year: '2003', name: 'LeBron James', rank: 1, age: 34},
                        {year: '2010', name: 'Kyrie Irving', rank: 1, age: 36},
                        {year: '2003', name: 'Dwyane Wade', rank: 5, age: 26}
                    ],
                    page:{
                        cur: 1,
                        size: 20,
                        total: 18
                    },
                    feedback: '',
                    handle: (row, type) => {
                        console.log(row.name, type)
                    },
                    handlePage: val => {
                        console.log(val);
                        this.tables.page.cur = val
                    },
                    handleSort: (prop, order) => {
                        console.log(prop, order)
                    }
                }
            }
        }
    }
</script>

<style>
    .bbb{
        color: red;
    }
    .aaa{
        color: blue;
        font-weight: bold;
    }
</style>