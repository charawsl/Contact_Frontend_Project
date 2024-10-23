
<template>
    <div>
        <el-container>
            <el-header>赖哥的通讯录</el-header>
            <el-main>

                    <!--搜索表单-->
                    <el-form :inline="true" :model="searchPeople" class="demo-form-inline">
                    <el-form-item label="姓名">
                        <el-input
                        v-model="searchPeople.name"
                        placeholder="请输入联系人姓名"
                        ></el-input>
                    </el-form-item>
                    <el-form-item label="性别">
                        <el-select v-model="searchPeople.gender" placeholder="请选择">
                        <el-option label="男" value="1"></el-option>
                        <el-option label="女" value="2"></el-option>
                        </el-select>
                    </el-form-item>


                    <el-form-item>
                        <el-button type="primary" @click="onSubmit">查询</el-button>
                        <el-button type="info" @click="clear">清空</el-button>
                    </el-form-item>
                    </el-form>

                    <!-- 添加人员按钮 -->
                     <el-row>
                        <el-button
                             style="float: center"
                            type="primary"
                             @click="dialogFormVisible = true; people = {id: '',name: '',phone: '',gender:'',address:''}"
                             >+ 添加联系人</el-button
                         >
                     </el-row>
                     <br>




                <el-table
                    :data="tableData"
                    height="620"
                    border
                    style="width: 100%">

                    
                    <el-table-column
                    prop="name"
                    label="姓名"
                    width="180">
                    </el-table-column>
                    <el-table-column
                    prop="phone"
                    label="电话号码">
                    </el-table-column>

                    <el-table-column
                    label="性别">
                    <template slot-scope="scope">
                        <span>
                        {{scope.row.gender == "1" ? "男" : "女"}}</span>
                    </template>
                    </el-table-column>

                    <el-table-column
                    prop="address"
                    label="地址">
                    </el-table-column>
                    <el-table-column
                    prop="createTime"
                    label="创建时间">
                    </el-table-column>
                    <el-table-column
                    prop="updateTime"
                    label="更新时间">
                    </el-table-column>

                    <el-table-column
                        label="删除">
                        <template slot-scope="scope">
                            <el-button type="primary" @click="update(scope.row.id)">编辑</el-button>
                            <el-button type="danger" @click="deleteById(scope.row.id)">删除</el-button>

                        </template>
                            
                    </el-table-column>
                </el-table>
            </el-main>
        </el-container>



        <!--添加数据对话框表单-->
    <el-dialog ref="form" title="编辑员工" :visible.sync="dialogFormVisible" width="30%">
      <el-form ref="form" :model="people" label-width="80px" size="mini">
        <el-form-item label="姓名">
          <el-input v-model="people.name"></el-input>
        </el-form-item>
        <el-form-item label="性别" >
          <el-select v-model="people.gender" placeholder="请选择" style="width:100%" >
             <el-option
              v-for="item in genderList"
              :key="item.value"
              :label="item.name"
              :value="item.id"
            />
          </el-select>
        </el-form-item>

        <el-form-item label="电话号码">
          <el-input v-model="people.phone"></el-input>
        </el-form-item>

        <el-form-item label="住址">
          <el-input v-model="people.address"></el-input>
        </el-form-item>




        <el-form-item>
          <el-button type="primary" @click="add()">提交</el-button>
          <el-button @click="dialogFormVisible = false">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
    <br>
    <!--分页工具条-->
    <div align = "center">
        <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :background="background"
        :current-page="currentPage"
        :page-sizes="[5, 10, 15, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="totalCount">
        
        </el-pagination>
    </div>
 </div>
</template>

<script>
import axios from 'axios';
export default{
    data () {
        return {
            background: true,
            // 每页显示的条数
            pageSize: 5,
            // 总记录数
            totalCount: 100,
            // 当前页码
            currentPage: 1,
            //表单数据
            people: {
                id: '',
                name: '',
                phone: '',
                gender:'',
                address:''
            },
            //表格数据
            tableData: [],
            genderList: [{id: 1,name: "男"},{id: 2,name: "女"}],
            //添加表格显示
            dialogFormVisible: false,
            searchPeople: {
                name: '',
                gender: '',
      },
        }
    },
    methods:{


        //删除部门
        deleteById(id) {
            axios.delete("http://121.37.25.182:8080/listPeople/" + id)
            .then(() => {
                this.page();
            })

        },
        init(){
            axios.get("http://121.37.25.182:8080/listPeople")
            .then(resp=>{
            this.tableData=resp.data.data; //响应数据赋值给数据模型
        });
        },
        //添加数据
        add() {
            if(this.people.id){
                axios.post("http://121.37.25.182:8080/updatePeople", this.people)
                .then(() => {
                this.page();
            })
            }
            else{
                axios.post("http://121.37.25.182:8080/listPeople", this.people)
                .then(() => {
                this.page();
            })
            }

        },
        page(){
            axios.get("http://121.37.25.182:8080/pageListPeople?page="+this.currentPage +"&pageSize="+this.pageSize+"&searchPeopleName="+this.searchPeople.name+"&searchPeopleGender="+this.searchPeople.gender)
            .then(resp=>{
            this.totalCount=resp.data.data.total; //响应数据赋值给数据模型
            this.tableData=resp.data.data.rows;
        });
        },
        handleSizeChange(val){
            this.pageSize = val;
            this.page();

        },
        handleCurrentChange(val){
            this.currentPage = val;
            this.page();

        },
        //查询方法
        onSubmit() {
            this.currentPage = 1;
            this.page();
        },
        //清空
        clear(){
            this.searchPeople = {name: "", gender: ""};
            this.page();
        },
        //更新数据
        update(id) {
        //1. 打开窗口
        this.dialogFormVisible = true;
        //2. 发送请求

            //axios.get("http://localhost:8080/selectById?id="+id)
            axios.get("http://121.37.25.182:8080/selectById/"+id)
            .then((result) => {
            if (result.data.code == 1) {
            this.people = result.data.data;
            }
        });
    },

    },
    mounted(){
        this.page();
    }
}

</script>

<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    /* line-height: 850px; */
  }
  
  
</style>