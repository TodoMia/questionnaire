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
            <div class="preview_box">
                <h2 class="questionnaire_title">{{questionnaire.title}}</h2>
                <div class="con">
                    <div class="subject" v-for="(item,index) in questionnaire.subjectlist">
                        <h4 class="subjecttitle">{{item.serial}}、<span v-if="item.optiontype == 'radio'">（单选）</span><span v-if="item.optiontype == 'checkbox'">（多选）</span>{{item.subjecttitle}}{{item.optiocselect}}</h4>
                        <!-- 多选题 -->
                        <el-checkbox-group v-model="item.optiocselect" v-if="item.optiontype == 'checkbox'">
                            <el-checkbox v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、{{op.optionscon}}</el-checkbox>
                        </el-checkbox-group>
                        <!-- 多选题 -->
                        <!-- 单选题 -->
                        <el-radio-group v-model="item.optiocselect" v-if="item.optiontype == 'radio'">
                            <el-radio v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、{{op.optionscon}}</el-radio>
                        </el-radio-group>
                        <!-- 单选题 -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'basetable',
    data() {
        return {
            questionnaire:{
                title:'满意度调查',
                subjectlist : []
            },
            tablelist:[
                { 
                    "serial": 1, "optiontype": "radio", "subjecttitle": "题目", 
                    "option1": "选项1", "option2": "选项2"
                }, 
                { 
                    "serial": 2, "optiontype": "checkbox", "subjecttitle": "题目2", 
                    "option1": "选项11", "option2": "选项22", "option3": "选项33" 
                } ,
                { 
                    "serial": 2, "optiontype": "radio", "subjecttitle": "题目2", 
                    "option1": "选项11", "option2": "选项22", "option3": "选项33" 
                },
                
            ]
        };
    },
    created() {
        for(var i=0;i<this.tablelist.length;i++){
            var title={}
            if(this.tablelist[i].optiontype == 'radio'){
                title.optiocselect=''
            }else{
                title.optiocselect=[]
            }
            
            title.serial=this.tablelist[i].serial
            title.optiontype=this.tablelist[i].optiontype
            title.subjecttitle=this.tablelist[i].subjecttitle
            var opt=[]
            for(var j=0; j<(Object.keys(this.tablelist[i]).length-3); j++){
                // var op={}
                var dictionaries="ABCDEFGHIJKLMNOPQRST"
                var label=dictionaries.charAt(j)
                // op.optionlabel=  label
                // op.optionscon = this.tablelist[i]['option'+(j+1)]
                var op={
                    optionlabel:label,
                    optionscon: this.tablelist[i]['option'+(j+1)]
                }
                opt.push(op)
            }
            // console.log(opt);
            title.option=opt
            this.questionnaire.subjectlist.push(title)
        }
    },
    methods: {

    }
};
</script>

<style scoped>

</style>
