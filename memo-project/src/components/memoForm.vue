<template>
  <div>
    
    <!-- <form> -->
    제목 : <input type ="text" size="80%" v-model="title" placeholder="제목을 입력해주세요."/>
    <br/>
    <br/>
    <textarea v-model="content" placeholder="메시지를 입력해주세요." rows="20" cols="100" align="center"></textarea>
    <br/>
    <!-- <button @click="addmemo" type="submit" align="left">등록</button> -->
    <button @click="addmemo" align="left">등록</button>
    <button @click="resetField" >reset</button>
    <!-- </form> -->
  </div>
</template>

<script>
export default {
  name: 'memoForm',
  data: function() {
    return {
      title : '',
      content : '',

    }
  },
  methods:{
      addmemo : function(){
          const {title, content} = this;
          // const id = new Date().getTime();
          const isEmpty = title.length <= 0 || content.length <= 0;
          if(isEmpty){
            return false;
          }
          // this.$emit('Paddmemo',{id,title,content});
          this.$emit('Paddmemo',{title, content});
          this.resetField();
      },

      resetField : function(){
        this.title = '';
        this.content = '';
      }
  }
}
</script>


<style scoped>
</style>
