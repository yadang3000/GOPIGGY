<template>
    <div>
        <add :title="ticketName" subtitle="注册赠x元优惠券" :ticketData="ticketData" :ticketColor="ticketColor" :ticketAdd="showDialog"></add>

        <el-dialog :title="'添加'+ticketName" v-model="dialogVisable" top="5%">
            <el-form :model="form" ref="form" :rules="rules" label-width="120px">
                <el-form-item label="标签名" prop="label">
                    <el-input v-model="form.label" auto-complete="off" placeholder="限制2-4字"></el-input>
                </el-form-item>
                <el-form-item label="活动标题" prop="title">
                    <el-input v-model="form.title" auto-complete="off" placeholder="限制2-100字"></el-input>
                </el-form-item>
                <el-form-item label="活动描述" prop="subtitle">
                    <el-input v-model="form.subtitle" auto-complete="off" placeholder="限制2-100字"></el-input>
                </el-form-item>
                <el-form-item label="优惠金额" prop="amount">
                    <el-input v-model="form.amount" type="number">
                        <span slot="prepend">购物车满</span>
                        <span slot="append">元</span>
                    </el-input>
                </el-form-item>
                <el-form-item label="抵扣金额" prop="deductible">
                    <el-input v-model="form.deductible" type="number">
                        <span slot="prepend">可抵扣</span>
                        <span slot="append">元</span>
                    </el-input>
                </el-form-item>
                <el-form-item label="优惠券总张数" prop="total">
                    <el-input v-model="form.total" type="number"></el-input>
                </el-form-item>
                <el-form-item label="每人限领" prop="limit">
                    <el-input v-model="form.limit" type="number"></el-input>
                </el-form-item>
                <el-form-item label="开始时间" prop="startTime">
                    <el-date-picker v-model="form.startTime" type="datetime" placeholder="选择日期时间">
                    </el-date-picker>
                </el-form-item>
                <el-form-item label="结束时间" prop="endTime">
                    <el-date-picker v-model="form.endTime" type="datetime" placeholder="选择日期时间">
                    </el-date-picker>
                </el-form-item>
                <el-form-item label="参加活动的商品">
                    <el-radio v-model="form.selectType" :label="1">全部商品</el-radio>
                    <el-radio v-model="form.selectType" :label="2">指定商品</el-radio>
                    <el-select v-model="form.selectedArticle" placeholder="输入商品名进行搜索" :disabled="form.selectType==1" multiple filterable class="block">
                        <el-option v-for="item in form.articleOptions" :key="item.value" :label="item.value" :value="item.id">
                            <span>{{ item.value }}</span>
                            <span class="p-l-5 f-color-grey f-10">ID: {{ item.id }}</span>
                        </el-option>
                    </el-select>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="closeDialog">取 消</el-button>
                <el-button type="primary" @click="saveDialog">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
    import add from './_创建活动.vue'
    export default {
        components: {
            add,
        },
        data() {
            return {
                ticketName: '全场赠券',
                ticketColor: 'blue',
                dialogVisable: false,

                // 已创建的活动
                ticketData: [],

                // 创建活动数据
                form: {
                    label: '',
                    title: '',
                    subtitle: '',
                    total: '',
                    startTime: '',
                    endTime: '',
                    amount: '',
                    deductible: '',
                    limit: '',
                    selectType: 1,
                    selectedArticle: [],
                    articleOptions: [{
                        value: 'Beijing',
                        id: '01'
                    }, {
                        value: 'Shanghai',
                        id: '02'
                    }, {
                        value: 'Nanjing',
                        id: '03'
                    }, {
                        value: 'Chengdu',
                        id: '04'
                    }],
                },

                // 表单验证规则
                rules: {
                    label: [{
                            required: true,
                            message: '请输入活动名称',
                            trigger: 'blur'
                        },
                        {
                            min: 3,
                            max: 4,
                            message: '长度在 3 到 4 个字符',
                            trigger: 'blur'
                        }
                    ],
                    title: [{
                        required: true,
                        message: '输入标题',
                        trigger: 'blur'
                    }],
                    subtitle: [{
                        required: true,
                        message: '输入简要描述',
                        trigger: 'blur'
                    }],
                    total: [{
                        required: true,
                        message: '输入数量',
                        trigger: 'blur'
                    }],
                    amount: [{
                        required: true,
                        message: '输入金额',
                        trigger: 'blur'
                    }],
                    deductible: [{
                        required: true,
                        message: '输入金额',
                        trigger: 'blur'
                    }],
                    limit: [{
                        required: true,
                        message: '输入数量',
                        trigger: 'blur'
                    }],
                    startTime: [{
                        type: 'date',
                        required: true,
                        message: '请选择开始时间',
                        trigger: 'change'
                    }],
                    endTime: [{
                        type: 'date',
                        required: true,
                        message: '请选择结束时间',
                        trigger: 'change'
                    }],
                }
            }
        },
        methods: {
            showDialog() {
                this.dialogVisable = true
                this.$nextTick(() => {
                    this.$refs['form'].resetFields()
                })
            },
            closeDialog() {
                this.dialogVisable = false
            },
            saveDialog() {
                this.$refs['form'].validate(valid => {
                    if (!valid) {
                        return false
                    }

                    let newData = {
                        label: this.form.label,
                        title: this.form.title,
                        subtitle: this.form.subtitle
                    }

                    this.ticketData.push(newData)
                    this.closeDialog()
                })
            },
        }
    }
</script>