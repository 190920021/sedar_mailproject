<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
        <script type="text/javascript" src="https://static.facilio.com/apps-sdk/latest/facilio_apps_sdk.min.js"></script>
<template >

    <form >
      
  <div style="margin-top:50px; margin-bottom:50px; margin-right:50px; margin-left:50px;"> 
    <div class="stick top">
      <h4 >Send RFQ email
        <el-button class="topbar" @click="close()">X</el-button>
      </h4>
    </div>
   <div>
    <strong>To</strong>
  </div><br>
  <el-input v-model="responce" clearable> </el-input><br><br>
  <div><strong>subject</strong>
  </div><br>
  <el-input placeholder="send RFQ Email" v-model="subject" clearable> </el-input><br><br>
  <div><strong>message</strong></div><br>
  <el-input type="textarea" rows="4" cols="100" style="height:60px;" v-model="content" clearable></el-input><br><br><br><br>
  <div class="stick bottom">
    <el-button type="default" style="margin-right:20px; float:right;" @click= "close()" >cancel</el-button>
    <el-button type="primary" @click="sendmail()" style="float:right; margin-right:20px; ">send</el-button>
  </div>
  </div>
</form>

</template>

<script >

export default {
  
  data() {
    return {
      
      responce:null,
      subject:"send RFQ Email",
      content:"",
      
    };
  },
  methods:{ 
    handleSubmit(){
      console.log(this.responce,this.subject,this.content)
    },
    async sendmail(){
                    let {responseCode, result } = await window.facilioApp.request.invokeFacilioAPI('/v2/workflow/runWorkflow', {
                    method: 'POST',
                    data: {
                    nameSpace: 'email',
                    functionName: 'vendors',
                    paramList: [this.responce,this.subject,this.content],               
    },})
    window.facilioApp.interface.trigger('hide');
    
    console.log(responseCode,result)
  },
    close(){
      window.facilioApp.interface.trigger('hide');
    },
  
  },
  mounted(){
                    window.facilioApp = FacilioAppSDK.init();
                    window.facilioApp.on('app.loaded', (data) => {
                    console.log('widget loaded')
                    console.log(data)
                    
                    let filters = {
                        resource: {
                            value1: [`${data.context.vendor.primaryContactEmail}`]
                        }
                    }

                    
                    console.log(filters.resource.value1)
                    this.responce=filters.resource.value1
                })

}

}
  
</script>


<style>
  .topbar{
    float:right; 
    border:0px;
    
  }
  .navbar {
  overflow: hidden;
  background-color: #333;
  position: fixed;
  top: 0;
  width: 100%;
}
  .stick{
    objectfit:contain;
    position: -webkit-sticky;
    background-color:white;
    position:sticky;
    display:block;
    width:100%;
    height: 35px;
    color:black;

  text-decoration: none;
  }
  .top{
    top : 0
  }
  .bottom{
    bottom: 0
  }
</style>






