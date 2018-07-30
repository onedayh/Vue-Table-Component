# Vue-Table-Component
一个基于element-ui（el-table）的vue（table）组件。
把你的表格数据、表头信息等参数传递给组件，即可生成你需要的表格。支持列排序（服务端）、对比同一列（仅支持'>'、'>'、'==='）数据可自定义样式、对每一行进行操作等功能。

## preview
![效果图片](http://m.qpic.cn/psb?/V13PFRwW2vD5Np/MjDQQlg9bpwNKIYCk7PDx0fQja1CkAPjWfhEdGSJAsg!/b/dC8BAAAAAAAA&bo=5QRTAwAAAAADB5M!&rf=viewer_4&t=5)

## options
######html
	<eg-table
        :tableData="tables.tableData"
        :options="tables.options"
        :page="tables.page"
        :feedback="tables.feedback"
        @handle="tables.handle"
        @handlePage="tables.handlePage"
        @handleSort="tables.handleSort"
    />
######js
	data(){
        return{
            tables: {
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
	            *   【slot】 => Array（非必填） => 对应单元格的操作
	            *              接受一个<<对象数组>>
	            *              字段说明：
	            *              【type】 => String（必填） => 操作类型
	            *              【name】 => String（必填） => 名字
	            *   【sort】 => String => 排序 custom
	            *   【center】 => Boolean => default: false => 为<true>时该列居中对齐, 默认局左对齐
	            *   【overflow】 => Boolean => default: false => 为<true>时内容溢出隐藏
	            */
                options: [
                    {prop: 'year', label: '日期'},
                    {prop: 'name', label: '姓名', overflow: true},
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
                tableData: [	// 表格数据
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
                    cur: 1,		// 当前页
                    size: 20,	// 每页数量
                    total: 18	// 总数
                },
                feedback: '',	//
                handle: (row, type) => {	// 操作
                    console.log(row.name, type)
                },
                handlePage: val => {		// 切换页码
                    console.log(val);
                    this.tables.page.cur = val
                },
                handleSort: (prop, order) => {	//	排序
                    console.log(prop, order)
                }
            }
        }
    }

