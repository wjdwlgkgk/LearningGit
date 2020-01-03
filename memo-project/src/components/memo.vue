<template>
  <div>
    <memoForm  @Paddmemo="Paddmemo"/>
    <view-memo v-for="memo in memos"
               :key="memo.id"
               :memo="memo"
               @deleteMemo="deleteMemo"
               @updateMemo="updateMemo"/>
    
  </div>
</template>

<script>

import memoForm from './memoForm.vue'
import viewMemo from './viewMemo.vue'
import axios from 'axios'

const memoAPICore = axios.create({
  baseURL : 'http://localhost:8000/api/memos'
});


export default {
  name:'memo',
  data(){
    return {
      memos:[],
    }
  },
  components:{
      memoForm,
      viewMemo
  },
  
  created(){
    // this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];
    memoAPICore.get('/').then(res => {
      this.memos = res.data;
    });
  },
  methods:{
      Paddmemo : function(memoForm){
        memoAPICore.post('/',memoForm).then(res => {
          this.memos.push(res.data);
        });

        // this.memos.push(memoForm);
        // const parsed = JSON.stringify(memos);
        // this.storeMemo();
        // localStorage.setItem('memos',parsed);
        // memo["title"] = memoForm.title;
        // memo["contents"] = memoForm.contents;

        // memos.put(memo);

        // localStorage.setItem('id',memoForm.id);
        // localStorage.setItem('id',memoForm.id);
        // alert("id : " + memoForm.id + ",\n title : " + memoForm.title + " ,\n contents : " + memoForm.contents);
        // Vue.nextTick().then(function () {})
        // alert("id : " + memo["id"] + ",\n title : " + memo["title"] + " ,\n contents : " + memo["contents"]);
        // this.contents.push(title);
        // this.memos.push(payload);
      },
      deleteMemo : function(id){
        const targetIndex = this.memos.findIndex(v => v.id === id); // findIndex 어떻게 쓰는건지 찾기. 
        memoAPICore.delete(`/${id}`).then(() => { // 먼저 백엔드 쪽에서 삭제 후 , 로컬에 있는 this.memos를 삭제. (이유 :완료되지 않을 수 있기 때문에.)
          this.memos.splice(targetIndex, 1);
        });
        // this.memos.splice(targetIndex, 1); // splice 어떻게 쓰는 건지 찾기. memos의 인덱스 memos[targetIndex]로부터 1개 인덱스 갯수 삭제. 그러니까 그냥 targetIndex를 삭제했다고보면됨.
        // this.storeMemo();
      },
      // storeMemo : function(){
      //   const memosToString = JSON.stringify(this.memos);
      //   localStorage.setItem('memos', memosToString);
      // },
      updateMemo : function(payload){
        const {id, content} = payload;
        const targetIndex = this.memos.findIndex(v => v.id === id);
        const targetMemo = this.memos[targetIndex];
        memoAPICore.put(`/${id}`, {content}).then(() =>{
        this.memos.splice(targetIndex,1, {...targetMemo, content: content}); // ...이란 무엇인가..?
        })
        // alert('targetMemo.id : ' + targetMemo.id + '\ntargetMemo.contents : ' + targetMemo.contents + '\ntargetMemo.title : ' + targetMemo.title + '\ntargetMemo : ' + targetMemo);
        
        // this.storeMemo();
      }
  }
}
</script>


<style scoped>
</style>

