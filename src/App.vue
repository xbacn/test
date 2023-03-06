/* eslint-disable vue/no-unused-components */
/* eslint-disable vue/no-unused-components */
<template>
  <div id="app">
    <div class="col-8 offset-2">
    
    <table class="table caption-top table-hover">
   
     <caption class="text-center">
       <h1>学生成绩管理系统</h1><br>
       <el-input type="text" class="w-25" aria-placeholder="请输入" v-model="searchname"></el-input><el-button @click="searchbyname">搜索</el-button><br>
       <button type="button" class="btn btn-primary" @click="getStudent">获取成绩信息</button>
       <button type="button" class="btn btn-primary" @click="dialogVisible = true">添加学生信息</button>
        <el-dialog
            title="提示"
            :visible.sync="dialogVisible"
            width="30%"
            :before-close="handleClose">
            <div>添加学生信息</div>
            <span slot="footer" class="dialog-footer">
              <el-button @click="dialogVisible = false">取 消</el-button>
              <el-button type="primary" @click="addStudent">添加</el-button>
            </span>
            <div><span>学号</span><input v-model="newStudent.number"/></div>
            <div><span>姓名</span><input v-model="newStudent.name"/></div>
            <div><span>年龄</span><input v-model.number="newStudent.age"/></div>
            <div><span>语文</span><input v-model.number="newStudent.chi"/></div>
            <div><span>数学</span><input v-model.number="newStudent.math"/></div>
            <div><span>英语</span><input v-model.number="newStudent.eng"/></div>
        </el-dialog>

        <!-- 注册 -->
        <el-button type="text" class="btn btn-primary" @click="centerDialogVisible = true"> 注册</el-button>
        <el-dialog
          title="提示"
          :visible.sync="centerDialogVisible"
          width="30%"
          center>
          <div class="text-center">
            <span>注册</span><tr></tr>
            <span>用户名</span><input v-model="user_forregister.username"/>
            <br>
            <span>密码</span><input v-model="user_forregister.password"/>
            <br>
            <span>确认密码</span><input v-model="user_forregister.confirmpassword"/>
            </div>
          <span slot="footer" class="dialog-footer">
            <el-button @click="centerDialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="register">确 定</el-button>
          </span>
        </el-dialog>
        
<!-- 登录 -->
        <el-button type="text" class="btn btn-primary" @click="centerDialogVisiblelogin = true"> 登录</el-button>
        <el-dialog
          title="提示"
          :visible.sync="centerDialogVisiblelogin"
          width="30%"
          center>
          <div class="text-center">
            <span>登录</span>
            <tr></tr>
            <span>用户名</span><input v-model="user.username"/>
            <br>
            <span>密码</span><input v-model="user.password"/>
            </div>
          <span slot="footer" class="dialog-footer">
            <el-button @click="centerDialogVisiblelogin = false">取 消</el-button>
            <el-button type="primary" @click="login">确 定</el-button>
          </span>
        </el-dialog>
      </caption>

      <thead>
        <tr>
          <th scope="nember">学号</th>
          <th scope="name">姓名</th>
          <th scope="age">年龄</th>
          <th scope="chi">语文</th>
          <th scope="math">数学</th>
          <th scope="eng">英语</th>
          <th scope="eng">操作</th>
        </tr>
      </thead>
      <tbody>
        <StudentTable v-for="stu in studentpage" :key="stu.id" :student='stu'></StudentTable>
        <!-- <student-table></student-table> -->
      </tbody>


     


    </table>
    <div class="text-center"> 
      <el-button-group>
      <el-button type="primary" icon="el-icon-arrow-left" @click="lastpage">上一页</el-button>
      <el-button type="primary" @click="nextpage">下一页<i class="el-icon-arrow-right el-icon--right" ></i></el-button>
      </el-button-group>
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import StudentTable from './components/StudentTable.vue'
// import { computed } from 'vue'

export default {

  name: 'App',
  
  components: {
    // eslint-disable-next-line vue/no-unused-components
    StudentTable
  },
  
  data() {
    return{
      searchname:"",
      students:[],
      page:1,//分页
      dialogVisible: false,
      centerDialogVisible: false,
      centerDialogVisiblelogin:false,
      newStudent:{
        number:"",
        name:"",
        age:"",
        chi:"",
        math:"",
        eng:"",
      },
      user:{
        username:"",
        password:"",
      },
      user_forregister:{
        username:"",
        password:"",
        confirmpassword:"",
      }
    }
  },
  methods:{
    getStudent(){
      if( sessionStorage.getItem("islogin")=="ture"){
      axios({
              url:'http://localhost:8081/showall',
              method:'Get',
            }).then(res=>{
              // console.log(res.data)
              this.students=res.data
            })
      }else{
        alert("请登录")
      }
      
    },
     handleClose(done) {
        this.$confirm('确认关闭？')
          .then(() => {
            done();
          })
          .catch(()=> {});
      },
      addStudent(){
        axios({
          url: 'http://localhost:8081/add',
          method: 'POST',
          data:this.newStudent
        })
        this.dialogVisible= false
      },
      lastpage(){
        this.page -= 1;
      },
      nextpage(){
        this.page += 1;
      },
      login(){          ///登录
        axios({
          url:'http://localhost:8081/login',
          method: 'POST',
          data:this.user,
          
        }).then(res=> {
          
          console.log(res.data)
          if(res.data=="1"){
            sessionStorage.setItem("islogin","ture")
          }
          else{
            alert("用户名或密码错误")
          }
        })
        this.centerDialogVisiblelogin = false
      },
      register(){
        axios({
          url:'http://localhost:8081/register',
          method: 'POST',
          data:this.user_forregister,
        })
        this.$alert("注册成功")
        this.centerDialogVisible = false
      },
      searchbyname(){
        axios({
          url:'http://localhost:8081/search',
          method: 'POST',
          data:{
            name:this.searchname,
          }
        }).then(res=>{
          this.students=res.data
        })
      }
      // toregist(){
      //       this.is_edit=true;
      //   },
  },
  computed:{
    studentpage(){
      return this.students.slice(this.page*5-5,this.page*5)
    }
  }
}
</script>

<style>


</style>
