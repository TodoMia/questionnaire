<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-lx-calendar"></i> 表单</el-breadcrumb-item>
                <el-breadcrumb-item>编辑器</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            
            <!-- <el-upload
                class="upload-demo"
                drag
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-change="filechange"
                :auto-upload="false"
                >
                <i class="el-icon-upload"></i>
                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
            </el-upload> -->
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
            <el-table
            :data="datalist"
            style="width: 100%">
            <el-table-column
              prop="姓名"
              label="姓名"
              width="180">
            </el-table-column>
            <el-table-column
              prop="邮件地址"
              label="邮箱"
              width="180">
            </el-table-column>
            <el-table-column
              prop="移动电话"
              label="电话">
            </el-table-column>
          </el-table>
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
                editorOption: {
                    placeholder: 'Hello World'
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
                        console.log(outdata)  // 最终解析出来的值
                        _this.datalist=outdata

                        var obj = {  
                        key1:1,  
                        key2:2,  
                        key3:3  
                        };  
                        
                        console.log(Object.getOwnPropertyNames(_this.datalist[0]))
                        console.log(Object.getOwnPropertyNames(_this.datalist[0]).sort())
                        console.log(Object.getOwnPropertyNames(_this.datalist[0]).length  )
                        console.log(Object.keys(_this.datalist[0]).length)
                        console.log(Object.getOwnPropertyNames(obj).length  )
                        console.log(Object.keys(obj).length)
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