<template>
    <div>

        <el-form :model="form" v-loading="listLoading" :label-position="labelPosition" label-width="180px" style="width:60%;">

            <el-form-item label="楼盘名称:">
                <el-input v-model="form.label"></el-input>
            </el-form-item>

            <el-form-item label="标签:">
                        <el-tag :key="tag" v-for="tag in dynamicTags" closable :disable-transitions="false" @close="handleClose(tag)">
                            {{tag}}
                        </el-tag>
                        <el-input class="input-new-tag" v-if="inputVisible" v-model="tagtValue" ref="saveTagInput" size="small" @keyup.enter.native="handleInputConfirm" @blur="handleInputConfirm">
                        </el-input>
                        <el-button v-else class="button-new-tag" size="small" @click="showInput">+新标签</el-button>

            </el-form-item>
            <el-form-item label="参考均价:">
                <el-input v-model="form.unit_price"></el-input>
                <text>元/㎡</text>

            </el-form-item>
            <el-form-item label="价格单位:">
                <el-radio-group v-model="form.unit_type">
                    <el-radio v-for="(item, index) of buildingPriceType" :key="index" :label="parseInt(index)">{{ item }}</el-radio>
                </el-radio-group>
            </el-form-item>

            <el-form-item label="建筑类型:">
                <!-- build_type -->
                <el-input v-model="form.build_type"></el-input>

            </el-form-item>

            <el-form-item label="建面:" required>

                <el-input v-model="square1" class="area"></el-input> ~

                <el-input v-model="square2" class="area"></el-input>&nbsp;m2

            </el-form-item>
            <el-form-item label="开盘时间:">
                <el-date-picker v-model="form.opening_time" type="date" placeholder="选择日期" value-format="timestamp">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="户型:">
                <!-- build_type -->
                <el-input v-model="form.huxing"></el-input>
            </el-form-item>

            <el-form-item label="首页展示:">
                <el-radio-group v-model="form.home_display">
                    <el-radio v-for="(item, index) of homeDisplay" :key="index" :label="parseInt(index)">{{ item }}</el-radio>
                </el-radio-group>
            </el-form-item>
            <el-form-item label="楼盘选择:">
                <el-radio-group v-model="form.type">
                    <el-radio v-for="(item, index) of buildingType" :key="index" :label="parseInt(index)">{{ item }}</el-radio>
                </el-radio-group>
            </el-form-item>

            <!-- <el-form-item label="明源云客项目id">
                <el-input v-model="form.cst_proj"></el-input>
            </el-form-item>
            <el-form-item label="保护期">
                <el-select v-model="form.deadline" placeholder="请选择保护期">
                    <el-option v-for="(item, index) in buildingdeadline" :key="index" :label="item" :value="parseInt(index)"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="楼盘来源">
                <el-select v-model="form.belong" placeholder="请选择楼盘来源">
                    <el-option v-for="(item, index) in belong" :key="index" :label="item" :value="parseInt(index)"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="签到流程">
                <el-select v-model="form.need_sign" placeholder="请选择签到流程">
                    <el-option v-for="(item, index) in process" :key="index" :label="item" :value="parseInt(index)"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="饭票宝展示">
                <el-select v-model="form.ticket" placeholder="请选择饭票宝">
                    <el-option v-for="(item, index) in ticket" :key="index" :label="item" :value="parseInt(index)"></el-option>
                </el-select>
            </el-form-item> -->

            <el-form-item label="销售员姓名:">
                <el-input v-model="form.salesperson"></el-input>
            </el-form-item>

            <el-form-item label="咨询电话:">
                <el-input v-model="form.hotline"></el-input>
            </el-form-item>
            <el-form-item label="楼盘图片:">

                <el-upload name="photo" class="upload-demo" :action="uploadImgUrl" :on-preview="handleContractPreview" :on-remove="handleContractRemove" :on-success="handlerContractSuccess" :multiple=true list-type="picture" :file-list="fileList">
                    <el-button size="small" type="primary">上传图片</el-button>
                    <div slot="tip" class="el-upload__tip">支持单个或批量上传</div>
                </el-upload>

            </el-form-item>

            <!-- <el-form-item label="楼盘列表图片:">
                <el-upload action="https://salesv2-ccw-test.colourlife.com/upload/image/5" list-type="picture-card" :auto-upload="true" name="photo" :limit="1" :on-exceed="handleExceed" :on-success="mySuccess">
                    <i slot="default" class="el-icon-plus"></i>
                    <div slot="file" slot-scope="{file}">
                        <img class="el-upload-list__item-thumbnail" :src="file.url" alt="">
                        <span class="el-upload-list__item-actions">
                            <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
                                <i class="el-icon-zoom-in"></i>
                            </span>
                            <span v-if="!disabled" class="el-upload-list__item-delete">
                                <i class="el-icon-download"></i>
                            </span>
                            <span v-if="!disabled" class="el-upload-list__item-delete">
                                <i class="el-icon-delete"></i>
                            </span>
                        </span>
                    </div>
                </el-upload>
            </el-form-item> -->
            <el-form-item label="楼盘首页图片:">
                <el-upload action="https://salesv2-ccw-test.colourlife.com/upload/image/450" list-type="picture-card" :limit="1" :on-exceed="handleExceed" :auto-upload="true" name="photo" :on-success="mySuccess2">
                    <i slot="default" class="el-icon-plus"></i>
                    <div slot="file" slot-scope="{file}">
                        <img class="el-upload-list__item-thumbnail" :src="file.url" alt="">
                        <span class="el-upload-list__item-actions">
                            <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
                                <i class="el-icon-zoom-in"></i>
                            </span>
                            <span v-if="!disabled" class="el-upload-list__item-delete">
                                <i class="el-icon-download"></i>
                            </span>
                            <span v-if="!disabled" class="el-upload-list__item-delete">
                                <i class="el-icon-delete"></i>
                            </span>
                        </span>
                    </div>
                </el-upload>
            </el-form-item>

            <el-form-item label="省市区:">
                <el-cascader size="large" :options="options" v-model="form.region_id" @change="handleChange">
                </el-cascader>
            </el-form-item>

            <el-form-item label="地址:">
                <el-input v-model="form.address"></el-input>
            </el-form-item>

            <el-form-item label="概述:">
                <el-input type="textarea" v-model="form.describe"></el-input>
            </el-form-item>

            <el-form-item label="交通:">
                <el-input type="textarea" v-model="form.transit"></el-input>
            </el-form-item>

            <el-form-item label="开盘状态:">
                <el-radio-group v-model="form.status">
                    <el-radio v-for="(item, index) of buildingStatus" :key="index" :label="parseInt(index)">{{ item }}</el-radio>
                </el-radio-group>
            </el-form-item>

            <el-form-item label="交楼时间:">
                <el-date-picker v-model="form.delivery_time" type="date" placeholder="选择日期" value-format="timestamp">
                </el-date-picker>
            </el-form-item>

            <el-form-item label="产权(年限):">
                <el-input v-model="form.term_for_use"></el-input>
            </el-form-item>

            <el-form-item label="推荐赚钱(元):">
                <el-input v-model="form.rebate"></el-input>
            </el-form-item>

            <el-form-item label="返还内容:">
                <el-input v-model="form.f_ticket"></el-input>
            </el-form-item>

            <el-form-item label="楼盘标签:">
                <el-input v-model="form.tag"></el-input>
            </el-form-item>

            <el-form-item label="">
                <el-button type="primary" @click="onSubmit">提交</el-button>
            </el-form-item>
        </el-form>
        <el-dialog title="图片预览:" :visible.sync="dialogVisible" width="65%" style="text-align:center;">
            <img :src="previewUrl" alt="" style="width:100%;height:auto;">
        </el-dialog>
    </div>
</template>

<script>
import httpClient from '@/utils/request'
import { staticUrl } from '@/utils/env'
import { regionDataPlus, CodeToText } from 'element-china-area-data'

export default {
    name: 'buildingEdit1',
    data() {
        return {
            dynamicTags: ['公交直达'], //后面改为quality
            tagtValue: '',
            inputVisible: false,
            square1: '',
            square2: '',
            uploadImgUrl: staticUrl + '/upload/image/650',
            labelPosition: 'right',
            disabled: false,
            listLoading: false,
            id: 0,
            building: '',
            buildingUseType: [],
            homeDisplay: [],
            buildingType: [],
            buildingPriceType: [],
            buildingStatus: [],
            options: regionDataPlus,
            form: {
                home_img:'test.com',
                quality:[],
                square:[],
                dynamicTags: ['公交直达'],
                tagtValue: '',
                sort: '',
                home_display: '',
                type: '',
                build_type: '',
                deadline: '',
                belong: '',
                need_sign: '',
                ticket: '',
                label: '',
                unit_price: '',
                unit_type: '',
                salesperson: '',
                hotline: '',
                images: [],
                address: '',
                describe: '',
                transit: '',
                status: '',
                delivery_time: '',
                opening_time: '',
                term_for_use: '',
                rebate: '',
                f_ticket: '',
                region_id: [],
                region_names: [],
                tag: '',
                cst_proj: '',
            },
            fileList: [],
            previewUrl: '',
            buildingdeadline: [],
            belong: [],
            process: [],
            ticket: [],
            dialogVisible: false,
        }
    },
    filters: {},
    created() {
        const params = this.$route.params
        this.id = parseInt(params.id)
        this.getData()
        this.getmykData()
    },
    methods: {
          handleExceed(){
          this.$message.warning("一次只能上传一张哦")
      },
 handlePictureCardPreview(file) { 
     this.dialogImageUrl = file.url
      this.dialogVisible = true 
      },

        mySuccess(res) {
            console.log('进到这里,已上传')
            console.log(res)
            this.imagesValue.push(res.content.photoPath)
        },
        mySuccess2(res) {//楼盘首页
            console.log('进到这里,已上传')
            console.log(res)
            this.form.home_img=res.content.photoPath
        },

     handleClose(tag) {
            this.dynamicTags.splice(this.dynamicTags.indexOf(tag), 1)
        },
        showInput() {
            this.inputVisible = true
            this.$nextTick(_ => {
                this.$refs.saveTagInput.$refs.input.focus()
            })
        },
        handleInputConfirm() {
            let tagtValue = this.tagtValue
            if (tagtValue) {
                this.dynamicTags.push(tagtValue)
            }
            this.inputVisible = false
            this.tagtValue = ''
        },

        getData() {
            this.listLoading = true
            httpClient.get('admin/building/' + this.id).then(response => {
                const data = response.content
                this.building = data.building
                this.buildingUseType = data.buildingUseType
                this.homeDisplay = data.homeDisplay
                this.buildingType = data.buildingType
                this.buildingPriceType = data.buildingPriceType
                this.buildingStatus = data.buildingStatus
                this.makeFormData()
                this.form.delivery_time =
                    this.form.delivery_time == null
                        ? ''
                        : this.form.delivery_time * 1000
                this.form.opening_time = this.form.opening_time * 1000
                this.form.images = this.dealFormPhoto(
                    this.building.images != ''
                        ? this.building.images.split(',')
                        : []
                )
                this.fileList = this.showPhoto(
                    this.building.images != ''
                        ? this.building.images.split(',')
                        : []
                )
                this.listLoading = false
                this.form.quality=[]
                this.form.square=[]
                this.form.home_img='test.com'//测试用
            })
        },
        getmykData() {
            this.listLoading = true
            httpClient.get('admin/build/select').then(response => {
                const data = response.content
                this.buildingdeadline = data.building_deadline
                this.belong = data.belong
                this.process = data.process
                this.ticket = data.ticket
                this.form.quality=[]
                this.form.square=[]
                this.form.home_img='test.com'//测试用
            })
        },
        makeFormData() {
            for (const key in this.form) {
                this.form[key] = this.building[key]
            }
        },
        dealFormPhoto(arr) {
            if (arr.length == 0) {
                return []
            }

            let newArr = []
            for (const i in arr) {
                if (arr[i] != '') {
                    // 去除oss 地址
                    // http://czy-version.oss-cn-shenzhen.aliyuncs.com/upload%2Fimages%2F20181228%2F1812281136105c259a2ac36ed.png?OSSAccessKeyId=LTAIMJIPmPad9RXY&Signature=7mJdF0D%2FBmiP8tr%2B1evRNU3amag%3D&Expires=1546054611
                    newArr.push(this.splitOssUrl(arr[i]))
                }
            }
            return newArr
        },
        splitOssUrl(url) {
            let ossUrl = decodeURIComponent(url)
            ossUrl = ossUrl.split('?')[0]
            ossUrl = ossUrl.split('upload/images/')[1]
            return 'upload/images/' + ossUrl
        },
        showPhoto(arr) {
            if (arr.length == 0) {
                return []
            }

            let newArr = []
            for (const i in arr) {
                if (arr[i] != '') {
                    newArr.push({
                        name: '图片' + (parseInt(i) + 1),
                        url: arr[i],
                    })
                }
            }
            return newArr
        },
        onSubmit() {
            // this.$message('提交中...')
            // 处理 region_names
            let arr = []
            for (const i in this.form.region_id) {
                if (this.form.region_id[i] == '') {
                    arr.push('')
                } else {
                    arr.push(CodeToText[this.form.region_id[i]] || '')
                }
            }
            this.form.region_names = arr;
            this.form.quality=this.dynamicTags;
            this.form.square.push(this.square1)
            this.form.square.push(this.square2)
            httpClient
                .post('admin/building/' + this.id, this.form)
                .then(response => {
                    if(response.code==0){
                          this.$message({
                        type: 'success',
                        message: '操作成功',
                    })
                    }
                  
                    // this.$router.go(-1)
                })
        },
        // 省市区
        handleChange(value) {
            console.log(value)
            // console.log(CodeToText[value[0]])
        },

        // 上传图片控件 - 合同
        previewPhoto(url) {
            this.previewUrl = url
            this.dialogVisible = true
        },

        handleContractRemove(file, fileList) {
            this.form.images = []
            for (const i in fileList) {
                if (fileList[i].hasOwnProperty('response')) {
                    this.form.images.push(
                        fileList[i].response.content.photoPath
                    )
                } else {
                    const path = fileList[i].url
                    const imgPath = this.splitOssUrl(path)
                    this.form.images.push(imgPath)
                }
            }
        },
        handleContractPreview(file) {
            this.previewPhoto(file.url)
        },
        handlerContractSuccess(response, fileList) {
            let data = response.content
            this.form.images.push(data.photoPath)
        },
    },
}


</script>

<style lang="css">
.img-responsive {
    width: 350px;
    height: auto;
}
.my-comtaint {
    position: relative;
}
.el-tag + .el-tag {
    margin-left: 10px;
}
.button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
}
.input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
}
.area {
    width: 80px;
}
</style>
