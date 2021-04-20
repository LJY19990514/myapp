<template>
    <div class="app-container">
        <!-- 图文 -->
        
        <el-button class="add-btn" icon="el-icon-edit"  type="primary">新增专栏</el-button>
      
        <el-select class="sel" v-model="value" placeholder="商品状态">
            <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
            </el-option>
        </el-select>
        <el-input class="ipt" v-model="input" placeholder="标题"></el-input>
        <el-button class="search-btn" icon="el-icon-search"  type="primary">搜索</el-button>
       
        <el-table
            class="tab"
            :data="tableData"
            
            border
            style="width: 100%">
            <el-table-column
                fixed
                prop="id"
                label="ID"
                width="80"
                align="center">
                </el-table-column>
            <el-table-column
                prop="content"
                label="图文内容"
                min-width="180px">
                <template slot-scope="{row}">
                    <div style="display: flex;">
                        <img :src="row.cover" style="width: 100px;height: 50px;margin-right: 10px;">
                        <div style="display: flex;flex-direction: column;justify-content: space-between;">
                            <span>{{row.title}}</span>
                            <span style="color:red;">￥{{row.price}}</span>      
                        </div>
                    </div>
                </template>
            </el-table-column>
            <el-table-column
                prop="sub_count"
                label="订阅量"
                width="120"
                align="center">
            </el-table-column>
            <el-table-column
                prop="state"
                label="状态"
                width="100">
            </el-table-column>
            <el-table-column
                prop="updated_time"
                label="创建时间"
                width="180">
            </el-table-column>
            <el-table-column
                fixed="right"
                label="操作"
                width="240">
            <template >
                <el-button type="primary" size="small">编辑</el-button>
                <el-button type="success" size="small">上架</el-button>
                <el-button type="danger" size="small">删除</el-button>
            </template>
            </el-table-column>
        </el-table>

        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :page-sizes="[10, 20, 30, 40]"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
        </el-pagination>
        
    </div>
</template>

<script>

import { fetchList } from "../../api/column"
let defaultQuery = {
    pageNum: 1, // 默认显示第几页面
    pageSize: 4, // 每页显示多少个
}
export default {

   
    data(){
        return {
            listQuery:Object.assign({},defaultQuery),
            tableData: [],
            options:[
                {
                    value:'1',
                    label:'已上架'
                },
                {
                    value:'2',
                    label:'已下架'
                }
            ],
            listLoading:false, //ajax请求数据的loading
            value:'',
            input:'',
            total:0,          //总数据条数
        }
    },
    created(){
        // 获取商品的列表数据
        this.getList();
    },


 methods: {
        // ajax获取商品列表
        getList(){
            this.listLoading = true;
            fetchList(this.listQuery).then((response) =>{
                console.log(response);
                if(response.code === 20000){
                    this.listLoading = false;
                    this.tableData = response.data.items;
                    this.total = response.data.total;
                }
            })
        },

        handleSizeChange(val) {
           // 每页多少条
            // console.log(`每页${val}条`);
            // this.listQuery.pageNum = 1;
            // this.listQuery.pageSize = val;
            // this.getList();  // 重新加载数据
        },
        handleCurrentChange(val) {
            // 点击切换页面
            // console.log(`当前页:${val}`);
            // this.listQuery.pageNum = val;
            // this.getList();  // 重新加载数据 
        }
    },
    
}
</script>

<style scoped>


.sel{
    width: 100px;
    position: fixed;
    right: 320px;
}
.ipt{
    width: 200px;
    position: fixed;
    right: 110px;
}
.search-btn{
    position: fixed;
    right: 20px;
}

.tab{
    margin-top: 30px;
}





</style>