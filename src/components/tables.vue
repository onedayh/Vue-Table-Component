<template>
    <div id="tables">
        <el-table
            header-row-class-name="table-header"
            :data="tableData"
            @sort-change="sortChange"
            style="width: 100%"
            empty-text=" "
        >
            <el-table-column
                v-for="(item, index) in options"
                v-if="item.hide ? Boolean(false) : Boolean(true)"
                :key="index"
                :prop="item.prop"
                :align="item.center ? 'center' : 'left'"
                :align-header="item.center ? 'center' : 'left'"
                :label="item.label"
                :show-overflow-tooltip="item.overflow ? Boolean(true) : Boolean(false)"
                :sortable="item.sort"
                min-width="50"
                :width="item.width"
            >
                <template slot-scope="scope">
                    <span :class="renderClass(scope.row[item.prop], item.render)">{{scope.row[item.prop]}}</span>
                    <el-button v-for="(it, idx) in item.slot" :key="idx" @click="handle(scope.row, it.type)" type="text" size="small">{{it.name}}</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="feedback" v-show="feedback">
            <i v-show="feedback === 'loading'" class='load-icon-item el-icon-loading'></i>
            <span v-if="feedback === 'error' || feedback === 'empty'">{{feedback === 'error' ? '获取数据失败' : feedback === 'empty' ? '暂无数据' : ''}}</span>
        </div>
        <div class="page-box" v-if="page.size !== 1000">
            <el-pagination
                @current-change="handlePage"
                :current-page.sync="page.cur"
                :page-size="page.size"
                layout="total, prev, pager, next"
                :total="page.total">
            </el-pagination>
        </div>
    </div>
</template>

<script>
    export default {
        name: "tables",
        props: {
            tableData: {        // 表格数据
                type: Array,
                default: () => {
                    return []
                }
            },
            /*
            *   表格配置【options】：接受一个<<对象数组>>
            *   字段说明：
            *   【hide】 => Boolean（非必填）=> default: false，为<true>时该列隐藏
            *   【label】 => String（必填） => 表头名称
            *   【prop】 => String（非必填） => 对应单元格的字段 => 有【slot】时不填，否则必填
            *   【render】 => Array（非必填） => 根据单元格内容给出对应的类名
            *              接受一个<<对象数组>>
            *              字段说明：
            *             【myTarget】 => String/Number（必填） => 比较的对象
            *             【myClass】 => String（必填） => 满足条件时的类名
            *             【mode】 => String（非必填） => 比较方法:
            *                                           'more' => 大于【myTarget】的值
            *                                           'less' => 小于【myTarget】的值
            *                                           不填【mode】 => 等于【myTarget】的值
            *    【slot】 => Array（非必填） => 对应单元格的操作
            *              接受一个<<对象数组>>
            *              字段说明：
            *              【type】 => String（必填） => 操作类型
            *              【name】 => String（必填） => 名字
            *     【sort】 => String => 排序 custom
            *     【center】 => Boolean => default: false => 为<true>时该列居中对齐, 默认局左对齐
            *     【overflow】 => Boolean => default: false => 为<true>时内容溢出隐藏
            */
            options: {
                type: Array,
                default: () => {
                    return []
                }
            },
            page: {        // 页数配置
                type: Object,
                default: () => {
                    return {
                        total: 0,       // 数据量
                        size: 20,       // 每页数据量
                        cur: 1          // 当前页
                    }
                }
            },
            feedback: {         // 用户体验反馈 => 'loading'-加载中， 'empty'-暂无数据,  'error'-获取数据失败
                type: String,
                default: ''
            }
        },

        methods: {
            // 操作
            handle(row, type){
                this.$emit('handle', row, type)
            },

            // 排序
            sortChange({prop, order}){
                this.$emit('handleSort', prop, order)
            },

            // 类名
            renderClass(target, renderList){
                let myClass = '', firstRender = false;
                if(!renderList) return;
                renderList.forEach(item => {
                    if(!item.mode && item.myTarget === target){
                        if(firstRender){
                            myClass += ' ' + item.myClass
                        }else{
                            myClass += item.myClass;
                            firstRender = true
                        }
                    }else if(item.mode === 'more' && target > item.myTarget){
                        if(firstRender){
                            firstRender = true;
                            myClass += ' ' + item.myClass
                        }else{
                            myClass += item.myClass;
                            firstRender = true;
                        }
                    }else if(item.mode === 'less' && target < item.myTarget){
                        if(firstRender){
                            myClass += ' ' + item.myClass
                        }else{
                            myClass += item.myClass;
                            firstRender = true;
                        }
                    }
                })
                return myClass
            },

            // 切换页数
            handlePage(val){
                this.$emit('handlePage', val);
            }
        }
    }
</script>

<style>
    #tables{
        position: relative
    }
    #tables .table-header{
        background-color: #f5f7fa;
        font-size: 13px;
        color: #6f7683;
        font-weight: bold
    }
    #tables .el-table{
        border: 1px solid #e6ebf5;
        border-bottom: none;
    }
    #tables .el-table th, #tables .el-table td{
        padding: 8px 0
    }
    #tables .el-table th{
        background-color: #f5f7fa
    }
    #tables .el-table th:first-child .cell{
        padding-left: 20px
    }
    #tables .el-table tbody tr td:first-child .cell{
        padding-left: 20px
    }
    #tables .caret-wrapper{
        height: 19px
    }
    #tables .sort-caret.ascending{
        top: -3px
    }
    #tables .sort-caret.descending{
        bottom: 0
    }
    #tables .el-table:before{
        z-index: 0
    }
    #tables .el-button--text{
        font-size: 13px;
        color: #60c0bd
    }
    #tables .el-button--small, #tables .el-button--small.is-round{
        padding: 0
    }
    #tables .feedback{
        height: 60px;
        line-height: 60px;
        position: absolute;
        top: 40px;
        width: 100%;
        text-align: center
    }
    #tables .feedback i{
        font-size: 20px
    }
    #tables .feedback span{
        font-size: 14px;
        color: #909399
    }
    #tables .page-box{
        height: 70px;
        padding-top: 30px;
        position: relative
    }
    #tables .page-box .el-pagination{
        float: right
    }
    #tables .page-box .el-pagination .el-pagination__total{
        position: absolute;
        left: 10px
    }
    #tables .page-box .el-pagination .el-pager li{
        min-width: 24px;
        height: 24px;
        line-height: 24px;
        padding: 0 6px;
        font-size: 12px;
        color: #555;
        border-radius: 4px;
        text-align: center;
        margin: 0 5px;
    }
    #tables .page-box .el-pagination .el-pager li.active{
        color: #60c0bd;
        background-color: #e7f6f5
    }
    #tables .page-box .el-pagination button .el-icon{
        margin-top: -2px;
    }
    #tables .page-box .el-pagination button .el-icon:before{
        font-size: 18px;
        color: #555;
        font-weight: normal
    }
    #tables .page-box .el-pagination button:disabled .el-icon:before{
        color: #e5e5e5
    }
</style>