<template>
    <div>
        <!-- 按钮 -->
        <el-button type="success" @click="toAddHandler">添加</el-button>
        <el-button type="danger" >删除</el-button>
        <!-- /按钮 -->
        <!-- 表格 -->
        <el-table :data="employees">
            <el-table-column fixed="left" prop="id" label="编号"></el-table-column>
            <el-table-column fixed="left" prop="username" label="用户名"></el-table-column>
            <el-table-column fixed="left" prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="age" label="年龄"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column width="120" prop="telephone" label="手机号"></el-table-column>
            <el-table-column width="200" prop="idCard" label="身份证号"></el-table-column>
            <el-table-column width="200" prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column fixed="right" label="操作">
                <!-- v-slot用于获取当前行数据 -->
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
                </template>
            </el-table-column>

        </el-table>
        <!-- /表格 -->
        <!-- 分页 -->
         <el-pagination layout="prev, pager, next" :total="50"></el-pagination>
        <!-- /分页 -->
        <!-- 模态框    冒号表示引用脚本-->
         <el-dialog :title="title" :visible.sync="visible" width="60%" >
             测试：{{form}}
         <el-form :model="form" label-width="80px">
             <el-form-item label="用户名">
                 <el-input v-model="form.username"/>
             </el-form-item>
             <el-form-item label="密码">
                 <el-input v-model="form.password" type="password"/>
             </el-form-item>
             <el-form-item label="手机号">
                 <el-input v-model="form.telephone"/>
             </el-form-item>
             <el-form-item label="姓名">
                 <el-input v-model="form.realname"/>
             </el-form-item>
             <el-form-item label="性别">
                 <el-radio-group v-model="form.gender">
    <el-radio label="男">男</el-radio>
    <el-radio label="女">女</el-radio>
  </el-radio-group>
             </el-form-item>
             <el-form-item label="身份证号">
                 <el-input v-model="form.idCard"/>
            </el-form-item>
            <el-form-item label="银行卡号">
                 <el-input v-model="form.bankCard"/>
             </el-form-item>
         </el-form>
         <span slot="footer" class="dialog-footer">
         <el-button @click="visible = false">取 消</el-button>
         <!-- @click=onclick -->
         <el-button type="primary" @click="submitHandler">确 定</el-button>
         </span>
        </el-dialog>
        <!-- /模态框 -->

    </div>
</template>

<script>
import request from '@/utils/request'  //第三方库
import querystring from 'querystring'  //系统库
//暴露接口
export default {
    // 存放网页中需要调用的方法
    data(){
        return {
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
                type:"waiter"
            }
        }
    },
     created(){
        //在页面加载出来的时候加载数据
        this.loadData()
    },
    methods:{
        //提交
       submitHandler(){
            let url = "http://loaclhost:6677/waiter/saveOrUpdate";
            //前端向后台发送请求，完成数据的保存操作
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                this.closeMethodHandler();
                this.loadData();
                this.$message({type:"success",message:response.message});
            })
        },
        //重载员工数据
        loadData(){
            //this指向vue实例。
            let url = "http://localhost:6677/waiter/findAll";
            request.get(url).then((response)=>{
                //箭头函数中的this指向外部函数中的this
                this.employees = response.data;
            })
        },
         

        toDeleteHandler(id){
            // 确认
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        
        },
        toUpdateHandler(row){
            this.title="修改员工信息";
            this.visible=true;
        },
       toAddHandler(){
           this.visible=true;
       }
    }
    //用于存放要在网页中存放的数据
   
}
</script>

<style scoped>
       
</style>