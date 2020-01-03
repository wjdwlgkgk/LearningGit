<template>
  <div>
    
    <li>
      <strong> {{memo.title}} </strong>
      <p @dblclick="handleDblClick">
        <template v-if = "!isEditing"> {{memo.content}} </template> <!-- template란 무엇인가? -->
        <input v-else 
        type = "text"
        ref = "content"
        :value = "memo.content"
        @blur="handleBlur" 
        @keydown.enter="updateMemo"/> <!-- keydown. 임 keydown: 아님. blur는 다른데 클릭할 때? 이것도 다시 알기  : 찾아보니 focusout과 같은..느낌 -->
      </p>
      <button type="button" @click="deleteMemo">x</button>
    </li>
    <!-- <h2>id : {{memo.id}} </h2>
    title : <input type ="text" size="80%" v-model="memo.title" placeholder="제목을 입력해주세요."/></br>
    contents : <textarea v-model="memo.contents" placeholder="메시지를 입력해주세요." rows="5" cols="30" align="center"></textarea> -->
  </div>
</template>

<script>
export default {
  name: 'viewMemo',
  data : function() {
    return {
      isEditing : false
    }
  },
  props : ['memo'],
  created(){
    
    // if(localStorage.getItem('memoForm')){
    //   try{
    //     this.memoForm = JSON.parse(localStorage.getItem('memoForm'));

    //   }catch(e){
    //     // localStorage.removeItem('memos');
    //   }
    // }
  },
  updated(){
    // if(localStorage.getItem('memoForm')){
    //   try{
    //     this.memoForm = JSON.parse(localStorage.getItem('memoForm'));
    //     const id = memoForm.id;
    //     const title = memoForm.title;
    //     const contents = memoForm.contents;
    //   }catch(e){
    //     localStorage.removeItem('memos');
    //   }
    // }


  },
  methods : {
    deleteMemo : function(){
      const id = this.memo.id;
      this.$emit('deleteMemo',id);
    },
    updateMemo : function(event){
      const id = this.memo.id;
      const content = event.target.value.trim();
      if(content.length <= 0){
        return false;
      }
      this.$emit('updateMemo',{id, content});
      this.isEditing = false;
    },
    handleDblClick : function(){
      this.isEditing = true;
      this.$nextTick(() => {
        this.$refs.content.focus();
      })
    },
    handleBlur : function (){
      this.isEditing = false;
    }
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
