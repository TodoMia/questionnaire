<template>
    <div class="tabs">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-cascades"></i> 模型
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <el-table
                :data="tableData"
                stripe
                border
                style="width: 100%">
                <el-table-column prop="title" label="问卷标题"width="180"></el-table-column>
                <el-table-column prop="name" label="创建人" width="120"></el-table-column>
                <el-table-column prop="date" label="创建时间" width="120"></el-table-column>
                <el-table-column prop="address" label="链接"></el-table-column>
                <el-table-column label="操作" width="240" align="center" fixed="right">
                    <template slot-scope="scope">
                        <el-button type="text" size="small">分享链接</el-button>
                        <el-button @click="handleClick(scope.row)" type="text" size="small">查看统计报告</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>
    </div>
</template>

<script>
export default {
    name: 'basetable',
    data() {
        return {
            tableData: [
                {
                date: '2016-05-02',
                name: '王小虎',
                title: '满意度调查',
                address: 'https://opensupport.alipay.com/support/knowledge/46919/201602503605#'
                }, {
                date: '2016-05-04',
                name: '王小虎',
                title: '调查报告',
                address: 'https://opensupport.alipay.com/support/knowledge/46919/201602503605#'
                }, {
                date: '2016-05-01',
                name: '王小虎',
                title: '投票评选',
                address: 'https://opensupport.alipay.com/support/knowledge/46919/201602503605#'
                }, {
                date: '2016-05-03',
                name: '王小虎',
                title: '学术调研',
                address: 'https://opensupport.alipay.com/support/knowledge/46919/201602503605#'
                }
            ]
        };
    },
    created() {
        
    },
    methods: {

    }
};
</script>

<style scoped>

</style>
