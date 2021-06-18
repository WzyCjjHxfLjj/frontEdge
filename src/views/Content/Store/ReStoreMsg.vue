<template>
  <div>
  <h2>我的信息</h2>
  <div class="material">
    <div class="material-item">
      <span class="attribute">商家号:</span>
      <span class="value">{{ s_id }}</span>
    </div>
    <div class="material-item">
      <span class="attribute">商家名:</span>
      <span class="value">{{ s_name }}</span>
      <el-button type="info" @click="dialogVisibleName = true" plain>编辑</el-button>
    </div>
    <div class="material-item">
      <span class="attribute">联系方式:</span>
      <span class="value">{{ s_tel }}</span>
      <el-button type="info" @click="dialogVisibleTel = true" plain>编辑</el-button>
    </div>
    <div class="material-item">
      <span class="attribute">商家地址:</span>
      <span class="value">{{ s_address }}</span>
      <el-button type="info" @click="dialogVisibleAddress = true" plain>编辑</el-button>
    </div>
  </div>
    <el-dialog title="修改商家名" :visible.sync="dialogVisibleName" width="30%" :before-close="handleClose">
      <el-form>
        <el-form-item>
          <el-form-item label="商家名">
            <el-input type="text" v-model="newName" style="width: 200px" placeholder="请输入新商家名"></el-input>
          </el-form-item>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisibleName = false">取 消</el-button>
          <el-button type="primary" @click="dialogVisibleName = false;NameEdit()">确 定</el-button>
        </span>
    </el-dialog>
    <el-dialog title="修改电话" :visible.sync="dialogVisibleTel" width="30%" :before-close="handleClose">
      <el-form>
        <el-form-item>
          <el-form-item label="电话">
            <el-input type="text" v-model="newTel" style="width: 200px" placeholder="请输入新电话"></el-input>
          </el-form-item>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisibleTel = false">取 消</el-button>
          <el-button type="primary" @click="dialogVisibleTel = false;TelEdit()">确 定</el-button>
        </span>
    </el-dialog>
    <el-dialog title="修改地址" :visible.sync="dialogVisibleAddress" width="30%" :before-close="handleClose">
      <el-form>
        <el-form-item>
          <el-form-item label="地址">
            <el-input type="text" v-model="newAddress" style="width: 200px" placeholder="请输入地址"></el-input>
          </el-form-item>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisibleAddress = false">取 消</el-button>
          <el-button type="primary" @click="dialogVisibleAddress = false;AddressEdit()">确 定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "ReStoreMsg",

  data(){
    return{
      s_id: '',
      s_name: '',
      s_tel: '',
      s_address: '',
      newName: '',
      newTel: '',
      newAddress: '',
      dialogVisibleName: false,
      dialogVisibleTel: false,
      dialogVisibleAddress: false
    }
  },

  beforeRouteEnter(to,from,next){
    next(vm => {
      console.log(vm.d_id);
      vm.StoreMsgForm();
    })
  },

  methods:{
    NameEdit(){
      this.dialogVisibleName = false;
      this.s_name = this.newName;
      this.StoreEditForm();
    },

    TelEdit(){
      this.dialogVisibleTel = false;
      this.s_tel = this.newTel;
      this.StoreEditForm();
    },

    AddressEdit(){
      this.dialogVisibleAddress = false;
      this.s_address = this.newAddress;
      this.StoreEditForm();
    },

    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },

    EditSuccess(){
      this.$message({
        showClose: true,
        message: '编辑成功',
        type: 'success',
        duration: 1000
      });
    },

    StoreMsgForm(){
      const tokenName = localStorage.getItem('tokenName');  /*从本地存储中取出tokenName的值*/
      const tokenValue = localStorage.getItem('tokenValue'); /*从本地存储中取出tokenValue的值*/
      const header = {
        "content-type": "application/x-www-form-urlencoded"
      };
      if(tokenName !== undefined && tokenName !== ''){
        header[tokenName] = tokenValue
      }
      this.s_id=this.$store.state.id;
      this.axios({
        method: 'get',
        url:'http://localhost:8081/store/info/'+this.s_id,
        headers: header,
      }).then((response)=>{
        console.log(response);
        if(response.data.code===0){
          this.s_name=response.data.data.store.name;
          this.s_tel=response.data.data.store.telephone;
          this.s_address=response.data.data.store.address;
        }
      })
        .catch((error)=>{
          console.log(error);   /*抓错*/
        });
    },

    StoreEditForm(){
      const tokenName = localStorage.getItem('tokenName');  /*从本地存储中取出tokenName的值*/
      const tokenValue = localStorage.getItem('tokenValue'); /*从本地存储中取出tokenValue的值*/
      const header = {
        "content-type": "application/x-www-form-urlencoded"
      };
      if(tokenName !== undefined && tokenName !== ''){
        header[tokenName] = tokenValue
      }
      this.s_id=this.$store.state.id;
      this.axios({
        method: 'get',
        url:'http://localhost:8081/store/edit/'+this.s_id+'/'+this.s_name+'/'+this.s_tel+'/'+this.s_address,
        headers: header,
      }).then((response)=>{
        console.log(response);
        if(response.data.code===0){
          this.EditSuccess();
        }
      })
        .catch((error)=>{
          console.log(error);   /*抓错*/
        });
    }
  }
}
</script>

<style scoped>
.material{
  font-size: 20px;
}

.material-item{
  margin-bottom: 20px;
}

.attribute{
  color: #555666;
  font-weight: 520;

}

.value{
  margin-left: 15px;
}

.el-button{
  margin-left: 15px;
}

.index{
  color: #555666;
}

.itempos{
  margin-left: 10px;
  position: relative;
  left: 20px;
  bottom: 28px;
}

.buttonpos{

}
</style>
