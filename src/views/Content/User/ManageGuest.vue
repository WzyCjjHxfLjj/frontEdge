<template>
  <div>
    <h2>管理顾客</h2>
    <el-table :data="GuestTable" style="width: 100%">
      <el-table-column label="顾客号">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column label="顾客名">
        <template slot-scope="scope">
          <span size="medium">{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column label="联系方式">
        <template slot-scope="scope">
              <span size="medium">{{ scope.row.telephone }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: "ManageGuest",
  data() {
    return {
      GuestTable: [],
    }
  },

  beforeRouteEnter(to,from,next){
    next(vm => {
      vm.GuestMsgForm();
    })
  },

  methods: {
    handleEdit(index, row) {
      row.telephone+='a';
      this.GuestEditForm(index);
    },

    handleDelete(index, row) {
      /*this.tableData.splice(index,1)*/
      this.GuestDeleteForm(index);
    },

    geSuccess(){
      this.$message({
        showClose: true,
        message: '编辑成功',
        type: 'success',
        duration: 1000
      });
    },

    /*列表显示请求*/
    GuestMsgForm(){
      const tokenName = localStorage.getItem('tokenName');  /*从本地存储中取出tokenName的值*/
      const tokenValue = localStorage.getItem('tokenValue'); /*从本地存储中取出tokenValue的值*/
      const header = {
        "content-type": "application/x-www-form-urlencoded"
      };
      if(tokenName !== undefined && tokenName !== ''){
        header[tokenName] = tokenValue
      }
      this.axios({
        method: 'get',
        url:'http://localhost:8081/guest/show',
        headers: header,
      }).then((response)=>{
        console.log(response);
        if(response.data.code===0){
            this.GuestTable=response.data.data.guest;
        }
      })
        .catch((error)=>{
          console.log(error);   /*抓错*/
        });
    },
    /*编辑请求*/
    GuestEditForm(index){
      const tokenName = localStorage.getItem('tokenName');  /*从本地存储中取出tokenName的值*/
      const tokenValue = localStorage.getItem('tokenValue'); /*从本地存储中取出tokenValue的值*/
      const header = {
        "content-type": "application/x-www-form-urlencoded"
      };
      if(tokenName !== undefined && tokenName !== ''){
        header[tokenName] = tokenValue
      }
      this.axios({
        method: 'get',
        url:'http://localhost:8081/guest/user/'+this.GuestTable[index].id+'/'+this.GuestTable[index].name+'/'+this.GuestTable[index].telephone,
        headers: header,
      }).then((response)=>{
        console.log(response);
        if(response.data.code===0){
          this.geSuccess()
        }
      })
        .catch((error)=>{
          console.log(error);   /*抓错*/
        });
    },

    GuestDeleteForm(index){
      const tokenName = localStorage.getItem('tokenName');  /*从本地存储中取出tokenName的值*/
      const tokenValue = localStorage.getItem('tokenValue'); /*从本地存储中取出tokenValue的值*/
      const header = {
        "content-type": "application/x-www-form-urlencoded"
      };
      if(tokenName !== undefined && tokenName !== ''){
        header[tokenName] = tokenValue
      }
      this.axios({
        method: 'get',
        url:'http://localhost:8081/guest/delete/'+this.GuestTable[index].id,
        headers: header,
      }).then((response)=>{
        console.log(response);
        if(response.data.code===0){
          this.geSuccess();
          this.GuestMsgForm();
        }
      })
        .catch((error)=>{
          console.log(error);   /*抓错*/
        });
    },

  }
}
</script>

<style scoped>

</style>
