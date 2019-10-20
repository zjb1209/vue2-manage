<template>
    <div class="fillcontain">
        <head-top></head-top>
        <el-row style="margin-top:20px">
             <el-col :span ="12" :offset="4">
                <el-form :model="formData" :rules="rules" ref="formData" label-width="110px" class="demo-formData">
                    <el-form-item label="人员编码" props="code">
                        <el-input placeholder="请输入人员编码"  v-model="formData.code"></el-input>
                    </el-form-item>
                    <el-form-item label="人员名称" props="name">
                        <el-input placeholder="请输入人员名称" v-model="formData.name"></el-input>
                    </el-form-item>
                    <el-form-item label="身份证号" props="idCard">
                        <el-input placeholder="请输入身份证号" v-model="formData.idCard"></el-input>
                    </el-form-item>
                    <el-form-item label="性别" props="sex">
                        <el-radio v-model="formData.radio" label="F">男</el-radio>
                        <el-radio v-model="formData.radio" label="M">女</el-radio>
                    </el-form-item>
                    <el-form-item label="人员归属">
                         <el-select v-model="formData.belong"  placeholder="请选择">
                                <el-option
                                    v-for="item in belong"
                                    :key="item.value"
                                    :label="item.label"
                                    :value="item.value">
                                </el-option>
                         </el-select>
                    </el-form-item>
                    <el-form-item label="手机号码" props="telephone">
                         <el-input placeholder="请输入手机号码" v-model="formData.telephone"></el-input>
                    </el-form-item>
                    <el-form-item label="上传人员头像">
                        <el-upload
                             class="avatar-uploader"
                             :action="baseUrl + 'v1/adding/employee'"
                             :show-file-list="false"
                             :on-success="uploadImg"
                             :before-upload="beforeImgUpload">
                            <img v-if="formData.image_path" :src="baseImgPath + formData.image_path" class="avatar">
                            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                        </el-upload>
                    </el-form-item>
                    <el-form-item label="备注" props="remark">
                         <el-input placeholder="请输入人员备注" v-model="formData.remark"></el-input>
                    </el-form-item>
                    <el-form-item class="button_submit" style="margin-left:40%">
                         <el-button type="primary" @click="submitForm('formData')">保存</el-button>
                         <el-button>返回</el-button>
                    </el-form-item>

                </el-form>
             </el-col>
        </el-row>
    </div>
</template>

<script>
	import headTop from '../components/headTop'
	import {baseUrl, baseImgPath} from '@/config/env'

    export default {
        data(){
            return{
				baseUrl,
				baseImgPath,
            	formData:{
					code:'',
					name:'',
					idCard:'',
					sex:'',
					telephone:'',
					belong:[],
					remark:''
                },
                rules:{
                    code:[
                    	{ required:true,message:'请输入人员编码',trigger:'blur'},
                    ],
                    name:[
                        { required:true,message:'请输入人员名称',trigger:'blur'},
                    ],
                    idCard:[
                        { required:true,message:'请输入人员身份证号',trigger:'blur'},
                    ],
                    telephone:[
                        { required:true,message:'请输入人员手机号码',trigger:'blur'},
						{ type: 'number', message: '电话号码必须是数字' },
                    ],
                    remark:[
                        { required:true,message:'请输入人员备注信息',trigger:'blur'}
                    ]
                },
				belong: [
					{
						value: '0',
						label: '正式人员'
					}, {
						value: '1',
						label: '临时人员'
					},{
						value: '2',
						label: '外包人员'
					},{
						value: '3',
						label: '实习人员'
					}],
            }
        },
		components: {
    		headTop,
    	},
		methods:{
        	uploadImg(res,img){
        		if(res.status == 1){
        			 this.formatData.image_path = res.image_path;
                }else{
        			this.$message.error('上传图片失败!!!')
                }
            },
            beforeImgUpload(file){
        		const isRightType = (file.type == 'image/jpeg') || (file.type == 'image/png');
        		const isLt2M  = file.size / 1024 /1024 <2;

        		if(!isRightType){
        			this.$message.error('上传头像图片只能是JPG格式');
                }
        		if(!isLt2M){
        			this.$message.error('上传头像图片大小不能超过2M');
                }
        		return isRightType && isLt2M;
            }
        }
    }
</script>
<style lang="less">
	@import '../style/mixin';
	.explain_text{
		margin-top: 20px;
		text-align: center;
		font-size: 20px;
		color: #333;
	}
</style>
