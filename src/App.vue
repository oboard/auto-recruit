<script setup>
import { reactive, ref } from 'vue';
import VueQr from 'vue-qr/src/packages/vue-qr.vue'
const url = ref('');
let list = reactive([

]);
const enter = (event) =>{
  fetch('/api/jobRequirementByEnterpriseDetail?entNo=' + url.value.split('id=')[1])  
  .then(  
    function(response) {  
      if (response.status !== 200) {  
        console.log('Looks like there was a problem. Status Code: ' +  
          response.status);  
        return;
      }
      // response.text().then((data)=>console.log(data));
      response.json().then(function(data) {  
        list.push(data.data);
        // list = [...list];
        console.log(list);
      });  
    }  
  )  
  .catch(function(err) {  
    console.log('Fetch Error :-S', err);  
  });
  url.value = '';
}
</script>

<template>
  <div>
    <input style="width:100%;height:64px" type="text" @keyup.enter="enter" v-model="url" />
    输入校招链接 如：<br />
    https://job.dgut.edu.cn/employment/zp_message/message_details/index.html?id=14311376<br />
    按下回车获得模板<br />
    <div v-for="l in list">
      <h1>{{ l.entName }}</h1>
      <br>
      <vue-qr :text="`https://job.dgut.edu.cn/employment/zp_message/message_details/index.html?id=${l.entId}`"
        :qid="l.entId"></vue-qr>
      <br>

      {{ l.entIntroduction }}
      <br>
      <br>


      <div v-for="item in l.jobRequirements">
        <b>{{ item.post }}</b>
        <br>
        地点：<b>{{ item.jobArea }}</b>
        <br>
        薪资待遇：<b>{{ item.salary }}</b>
        <br>
        <b>岗位要求：</b>
        <pre>{{ item.jobRequire }}</pre>

        <br>招聘截止时间：{{ item.endDate }}
        <br>
        <br>
      </div>
      <br>
      <br>
      <br>
    </div>
  </div>
</template>

<style scoped></style>
