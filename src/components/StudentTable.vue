<template>

  <tr>
        <!-- <th scope="row">1</th>
        <td>Mark</td>
        <td>Otto</td>
        <td>@mdo</td>
        <td>Mark</td>
        <td>Otto</td>
        <td>@mdo</td> -->
        <td v-show="!is_edit">{{ mystudent.number }}</td>
        <td v-show="!is_edit">{{ mystudent.name }}</td>
        <td v-show="!is_edit">{{ mystudent.age }}</td>
        <td v-show="!is_edit">{{ mystudent.chi }}</td>
        <td v-show="!is_edit">{{ mystudent.math }}</td>
        <td v-show="!is_edit">{{ mystudent.eng }}</td>
            <td v-show="!is_edit">
            <button type="button" class="btn btn-primary"  @click="xiugai">修改</button>
            </td>
            <!-- <td v-show="!is_edit"><button type="button" class="btn btn-primary">删除</button></td> -->
            <td v-show="!is_edit"><el-button type="danger" @click="deletestudcent">删除</el-button></td>
            <td v-show="is_edit"><input  class="w-50" type="text" v-model="mystudent.number"/></td>
            <td v-show="is_edit"><input  class="w-50" type="text" v-model="mystudent.name"/></td>
            <td v-show="is_edit"><input   class="w-50" type="text" v-model.number="mystudent.age"/></td>
            <td v-show="is_edit"><input  class="w-50" type="text" v-model.number="mystudent.chi"/></td>
            <td v-show="is_edit"><input   class="w-50" type="text" v-model.number="mystudent.math"/></td>
            <td v-show="is_edit"><input  class="w-50" type="text" v-model.number="mystudent.eng"/></td>

        <td v-show="is_edit">
          <el-button type="primary" @click="save">保存</el-button>


      </td>
      <td v-show="is_edit"><el-button type="danger" @click="deletestudcent">删除</el-button></td>

        
        </tr>
</template>


<script>
import axios from 'axios'

export default {
   props:["student"],
    data() {
        return {
            is_edit:false,
            mystudent:this.student,
        }
    },
    watch: {
      student(val) {
        this.mystudent = val;//新增result的watch，监听变更并同步到myResult上
      }
    },

    
    methods:{
        xiugai(){
            this.is_edit=true;
        },
        save(){
            axios({
                url: "http://localhost:8081/update",
                method: "POST",
                data:this.mystudent
            })
            this.is_edit=false;
        },
        deletestudcent(){
            axios({
                url: "http://localhost:8081/delete",
                method: "POST",
                data:this.mystudent
            })
            //刷新页面
            location.reload();//删不掉的话注释掉这一行
        },
    }

    // methods:{
    // delete(){
    //    axios({
    //         url:'http://localhost:8081/showall',
    //         method:'GET',
    //         data:this.student
    //     })
    // }

// }

}
</script>


<style>

</style>