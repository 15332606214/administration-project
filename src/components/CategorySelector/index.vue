<template>
    <div>
        <el-form :inline="true" :model="cForm" class="demo-form-inline" :disabled="!isShowList">
            <el-form-item label="一级分类">
                <!-- 收集的是选中的id -->
                <el-select v-model="cForm.category1Id" placeholder="请选择" @change="changeCategory1">
                    <el-option :label="c1.name" :value="c1.id" v-for="c1 in category1List" :key="c1.id"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="二级分类">
                <!-- 收集的是选中的id -->
                <el-select v-model="cForm.category2Id" placeholder="请选择" @change="changeCategory2">
                    <el-option :label="c2.name" :value="c2.id" v-for="c2 in category2List" :key="c2.id"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="三级分类">
                <!-- 收集的是选中的id -->
                <el-select v-model="cForm.category3Id" placeholder="请选择" @change="changeCategory3">
                    <el-option :label="c3.name" :value="c3.id" v-for="c3 in category3List" :key="c3.id"></el-option>
                </el-select>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    name: 'CategorySelector',
    props:['isShowList'],
    data() {
        return {
            cForm: {
                category1Id: '',
                category2Id: '',
                category3Id: ''
            },
            category1List:[],
            category2List:[],
            category3List:[]
        }
    },
    mounted(){
        this.getCategory1List()
    },
    methods:{
        // 获取一级分类
        async getCategory1List(){
            const result = await this.$API.category.getCategory1()
            if(result.code===200){
                this.category1List=result.data
            }
        },
        // 根据选中的一级分类获取二级分类
        async changeCategory1(category1Id){
            // 清空2,3数据
            this.cForm.category2Id=''
            this.cForm.category3Id=''
            this.category2List=[]
            this.category3List=[]

            // 组件通信把id传入父组件
            this.$emit('changeCategory',{categoryId:category1Id,level:1})
            
            // 发请求
            const result = await this.$API.category.getCategory2(category1Id)
            if(result.code===200){
                this.category2List=result.data
            }
           
        },
        // 根据二级分类获取三级分类
        async changeCategory2(category2Id){
            // 清空3数据
            this.cForm.category3Id=''
            this.category3List=[]

            // 组件通信把id传入父组件
            this.$emit('changeCategory',{categoryId:category2Id,level:2})
            
            // 发请求
            const result = await this.$API.category.getCategory3(category2Id)
            if(result.code===200){
                this.category3List=result.data
            }
        },
        // 选中三级分类要做的事
        changeCategory3(category3Id){
            // 组件通信把id传入父组件
            this.$emit('changeCategory',{categoryId:category3Id,level:3})
        }
    }
}
</script>

<style>

</style>