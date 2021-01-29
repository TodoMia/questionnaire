<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 表单</el-breadcrumb-item>
                <el-breadcrumb-item>编辑器</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="list">
                <h2 class="questionnaire_title">{{questionnaire.title}}</h2>
                <div class="con">
                    <div class="subject" v-for="(item,index) in questionnaire.subjectlist">
                        <h4 class="subjecttitle">{{item.serial}}、<span v-if="item.optiontype == 'radio'">（单选）</span><span v-if="item.optiontype == 'checkbox'">（多选）</span><input class="input" v-model="item.subjecttitle" type="text" >{{item.optiocselect}}</h4>
                        <!-- 单选题 -->
                        <el-radio-group v-model="item.optiocselect" v-if="item.optiontype == 'radio'">
                            <el-radio v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、<input class="input" v-model="op.optionscon" type="text" ></el-radio>
                        </el-radio-group>
                        <!-- 单选题 -->
                        <!-- 多选题 -->
                        <el-checkbox-group v-model="item.optiocselect" v-if="item.optiontype == 'checkbox'">
                            <el-checkbox v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、<input class="input" v-model="op.optionscon" type="text" ></el-checkbox>
                        </el-checkbox-group>
                        <!-- 多选题 -->
                        <!-- 添加选项 -->
                        <div>
                            <el-button type="text" @click="addoption(index)">添加选项</el-button>
                        </div>
                        <!-- 添加选项 -->
                        
                    </div>
                    <div class="edit">
                        <!-- <quill-editor ref="myTextEditor" v-model="content" :options="editorOption"></quill-editor> -->
                        <!-- <el-button class="editor-btn" type="primary" @click="submit">提交</el-button> -->
                        <el-button class="editor-btn" type="primary" @click="addradio">添加单选题</el-button>
                        <el-button class="editor-btn" type="primary" @click="addcheck">添加多选题</el-button>
                        <el-button class="editor-btn" type="primary" @click="preview">问卷预览</el-button>
                        <!-- <el-button class="editor-btn" type="primary" @click="preview2">打印</el-button> -->
                        
                    </div>

                    
                </div>
            </div>

            <!-- 预览 -->
            <el-dialog :title="revise_title" :visible.sync="preview_show"   width="80%">
            <!-- <div class="preview" > -->
                <div class="preview_box">
                    <h2 class="questionnaire_title">{{questionnaire.title}}</h2>
                    <div class="con">
                        <div class="subject" v-for="(item,index) in questionnaire.subjectlist">
                            <h4 class="subjecttitle">{{item.serial}}、<span v-if="item.optiontype == 'radio'">（单选）</span><span v-if="item.optiontype == 'checkbox'">（多选）</span>{{item.subjecttitle}}{{item.optiocselect}}</h4>
                            <!-- 单选题 -->
                            <el-radio-group v-model="item.optiocselect" v-if="item.optiontype == 'radio'">
                                <el-radio v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、{{op.optionscon}}</el-radio>
                            </el-radio-group>
                            <!-- 单选题 -->
                            <!-- 多选题 -->
                            <el-checkbox-group v-model="item.optiocselect" v-if="item.optiontype == 'checkbox'">
                                <el-checkbox v-for="(op,index2) in item.option" :label="op.optionlabel">{{op.optionlabel}}、{{op.optionscon}}</el-checkbox>
                            </el-checkbox-group>
                            <!-- 多选题 -->
                            
                        </div>
                        <div class="edit">
                            <el-button class="editor-btn" type="primary" @click="previewno">取消</el-button>
                            <el-button class="editor-btn" type="primary" @click="preview">提交问卷</el-button>
                        </div>
                    </div>
                </div>
            <!-- </div> -->
            </el-dialog>
        </div>
    </div>
</template>

<script>
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.snow.css';
    import 'quill/dist/quill.bubble.css';
    export default {
        name: 'editor',
        data: function(){
            return {
                revise_title:'问卷预览',
                preview_show:false,
                checkList:[],
                se:'',
                se2:'',
                questionnaire:{
                    title:'满意度调查',
                    subjectlist:[
                        {//第一题
                            serial:'1',
                            subjecttitle:'你所在的年级',
                            optiontype:'radio',
                            optiocselect:'',
                            option:[ //选项
                                {
                                    optionlabel:'A',
                                    optionscon:'备选项1'
                                },
                                {
                                    optionlabel:'B',
                                    optionscon:'备选项2'
                                }
                            ]
                        },
                        {//第二题
                            serial:'2',
                            subjecttitle:'你所在的年级',
                            optiontype:'radio',
                            optiocselect:'',
                            option:[ //选项
                                {
                                    optionlabel:'A',
                                    optionscon:'选项3'
                                },
                                {
                                    optionlabel:'B',
                                    optionscon:'选项4'
                                }
                            ]
                        },
                        {//第三题
                            serial:'3',
                            subjecttitle:'你所在的年级',
                            optiontype:'checkbox',
                            optiocselect:[],
                            option:[ //选项
                                {
                                    optionlabel:'A',
                                    optionscon:'选项3'
                                },
                                {
                                    optionlabel:'B',
                                    optionscon:'选项4'
                                }
                            ]
                        }
                    ]
                },
                content: '',
                editorOption: {
                    placeholder: 'Hello World'
                }
            }
        },
        methods: {
            preview2(){
               this.$router.push({path:'/icon'})  
            },
            // 预览
            previewno(){
                this.preview_show=false
            },
            preview(){
                this.preview_show=true
            },
            onEditorChange({ editor, html, text }) {
                this.content = html;
            },
            submit(){
                console.log(this.content);
                this.$message.success('提交成功！');
            },
            addoption(index){
                console.log(this.questionnaire.subjectlist[index].option.length)
                var id=this.questionnaire.subjectlist[index].option.length
                var dictionaries="ABCDEFGHIJKLMNOPQRST"
                var label=dictionaries.charAt(id)
                console.log(label)
                var newoption={
                            optionlabel:label,
                            optionscon:'选项'
                        }
                this.questionnaire.subjectlist[index].option.push(newoption)
            },
            addradio(){
                console.log(this.questionnaire.subjectlist.length)
                var newradio={
                    serial: this.questionnaire.subjectlist.length+1,
                    subjecttitle:'题目',
                    optiontype:'radio',
                    optiocselect:'',
                    option:[ //选项
                        {
                            optionlabel:'A',
                            optionscon:'备选项1'
                        },
                        {
                            optionlabel:'B',
                            optionscon:'备选项2'
                        }
                    ]
                }
                this.questionnaire.subjectlist.push(newradio)
                console.log(newradio)
            },
            addcheck(){
                var newradio={
                    serial: this.questionnaire.subjectlist.length+1,
                    subjecttitle:'题目',
                    optiontype:'checkbox',
                    optiocselect:[],
                    option:[ //选项
                        {
                            optionlabel:'A',
                            optionscon:'备选项1'
                        },
                        {
                            optionlabel:'B',
                            optionscon:'备选项2'
                        }
                    ]
                }
                this.questionnaire.subjectlist.push(newradio)
            }
        }
    }
</script>
<style scoped>
    .editor-btn{
        margin-top: 20px;
    }
    .questionnaire_title{
        text-align: center;
        padding: 10px 0;
    }
    .subject{
        padding: 5px 0;
    }
    .subjecttitle{
        margin-bottom: 5px;
    }
    .input{
        outline: none;
        /* border: 1px solid #ddd; */
        border: 0;
        padding: 5px;
        background-color: white;
    }
    .container{
        position: relative;
    }
    .preview{
        box-shadow: 0 0 5px #ddd; 
        box-sizing: border-box;
        width: 842px;
        height: 660px;
        padding: 20px 20px 40px 40px;
        /* background-color: rgb(253, 220, 220); */
        background-color: white;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        z-index: 1000;
    }
    .preview_box{
        width: 100%;
        height: 100%;
        /* overflow-y: auto; */
        padding: 0 20px;
        /* background-color: teal; */
    }
    .el-radio-group,.el-checkbox-group{
        width: 100%;
    }
    .el-radio,.el-checkbox{
        margin: 3px 10px;
        /* max-width: 20%; */
        min-width: 20%;
        /* background-color: tomato; */
    }
</style>