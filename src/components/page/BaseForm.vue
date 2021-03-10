<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-pie-chart"></i> 调查报告
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div >
                <h2>{{report.title}}</h2>
                <div v-for="(item,index) in report.subjectlist" class="queitem">
                    <h4>{{item.serial}}、{{item.subjecttitle}}</h4>
                    <div class="equchar">
                        <el-table
                        :data="item.option"
                        style="width: 100%">
                        <el-table-column
                        prop="optionscon"
                        label="选项"
                        width="300">
                        </el-table-column>
                        <el-table-column
                        prop="selectnumb"
                        label="小计"
                        width="100">
                        </el-table-column>
                        <el-table-column
                        label="比例">
                        <template slot-scope="scope">
                            <el-progress :text-inside="true" :stroke-width="26" :percentage="(scope.row.selectnumb/item.answertotal).toFixed(3)*100"></el-progress>
                        </template>
                        </el-table-column>
                    </el-table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'basecharts',
    data() {
        return {
            report:{
                queid:'12',
                title:'调查报告',
                subjectlist:[
                    {//第一题
                        serial: '1',
                        subjecttitle: '你所在的年级',
                        optiontype: 'radio',
                        optiocselect: '',
                        answertotal: 7189,
                        option:[ //选项
                            {
                                optionlabel:'A',
                                optionscon:'备选项1备选项1备选项1备选项1备选项1',
                                selectnumb: 4418
                            },
                            {
                                optionlabel:'B',
                                optionscon:'备选项2',
                                selectnumb: 1286
                            }
                        ]
                    },
                     {//第一题
                        serial: '2',
                        subjecttitle: '你满意吗',
                        optiontype: 'radio',
                        optiocselect: '',
                        answertotal: 7189,
                        option:[ //选项
                            {
                                optionlabel:'A',
                                optionscon:'满意',
                                selectnumb: 4418
                            },
                            {
                                optionlabel:'B',
                                optionscon:'一般',
                                selectnumb: 1186
                            },
                            {
                                optionlabel:'c',
                                optionscon:'不满意',
                                selectnumb: 186
                            }
                        ]
                    },
                ],
            }
        };
    }
};
</script>

<style scoped>
.container h2{
    text-align: center;
}
.container h4{
    margin: 5px 0;
}
.queitem{
    margin: 10px 0;
}
.equchar{
    margin: 10px;
}
</style>