<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 表单</el-breadcrumb-item>
                <el-breadcrumb-item>编辑器</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <el-upload
                class="upload-demo"
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview"                
                :limit="3"
                :file-list="fileList"
                :on-change="filechange"
            >
            <el-button size="small" type="primary">点击上传</el-button>
            </el-upload>
            <div>{{datalist}}</div>
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
                    </div>
                </div>
        </div>
    </div>
</template>

<script>
    // npm i xlsx --save-dev 
    import XLSX from "xlsx"; 
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.snow.css';
    import 'quill/dist/quill.bubble.css';
    import { quillEditor } from 'vue-quill-editor';
    export default {
        name: 'editor',
        data: function(){
            return {
                datalist:[],
                fileList:[],
                content: '',
                questionnaire:{
                    title:'满意度调查',
                    subjectlist : []
                }
            }
        },
        components: {
            quillEditor
        },
        methods: {
            handleRemove(){

            },
            handlePreview(){

            },
            filechange(file, fileList){
                this.questionnaire.title=file.name.substring(0, file.name.indexOf("."))
                var _this=this
                this.file = file.raw;
                var rABS = false; //是否将文件读取为二进制字符串
                var f = this.file;
                var reader = new FileReader();
                FileReader.prototype.readAsBinaryString = function (f) {
                    var binary = "";
                    var rABS = false; //是否将文件读取为二进制字符串
                    var pt = this;
                    var wb; //读取完成的数据
                    var outdata;
                    var reader = new FileReader();
                    reader.onload = function (e) {
                        var bytes = new Uint8Array(reader.result);
                        var length = bytes.byteLength;
                        for (var i = 0; i < length; i++) {
                            binary += String.fromCharCode(bytes[i]);
                        }
                        var XLSX = require('xlsx');
                        if (rABS) {
                            wb = XLSX.read(btoa(fixdata(binary)), { //手动转化
                                type: 'base64'
                            });
                        } else {
                            wb = XLSX.read(binary, {
                                type: 'binary'
                            });
                        }
                        // outdata就是你想要的东西 excel导入的数据
                        outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]);
                        // excel 数据再处理
                        // console.log(outdata)  // 最终解析出来的值
                        _this.datalist=outdata
                        var qulist=[]
                        for(var i=0;i<outdata.length;i++){
                            var title={}
                            if(outdata[i].optiontype == 'radio'){
                                title.optiocselect=''
                            }else{
                                title.optiocselect=[]
                            }
                            title.serial=outdata[i].serial
                            title.optiontype=outdata[i].optiontype
                            title.subjecttitle=outdata[i].subjecttitle
                            var opt=[]
                            for(var j=0; j<(Object.keys(outdata[i]).length-3); j++){
                                var dictionaries="ABCDEFGHIJKLMNOPQRST"
                                var label=dictionaries.charAt(j)
                                var op={
                                    optionlabel:label,
                                    optionscon: outdata[i]['option'+(j+1)]
                                }
                                opt.push(op)
                            }
                            title.option=opt
                            qulist.push(title)
                        }
                        // 判断对象有多少个属性
                        _this.questionnaire.subjectlist=qulist
                    }
                    reader.readAsArrayBuffer(f);
                }
                if (rABS) {
                    reader.readAsArrayBuffer(f);
                } else {
                    reader.readAsBinaryString(f);
                }

            },
            submitUpload() {
                this.$refs.upload.submit();
            },
            onEditorChange({ editor, html, text }) {
                this.content = html;
            },
            submit(){
                console.log(this.content);
                this.$message.success('提交成功！');
            }
        }
    }
</script>
<style scoped>
    .editor-btn{
        margin-top: 20px;
    }
    .container >>> .el-upload--text {
        background-color: #fff;
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        width: auto;
        height: auto;
        text-align: center;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
</style>