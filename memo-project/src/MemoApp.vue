<template>
    <div class="memo-app">
        <memo-form @addMemo="addMemo"/>
        <ul class ="memo-list">
            <memo v-for="memo in memos"
            :key = "memo.id"
            :memo = "memo"
            @deleteMemo="deleteMemo"
            @updateMemo="updateMemo"/>
        </ul>
    </div>
</template>

<script>
import MemoForm from './MemoForm';
import Memo from './Memo';
import axios from 'axios';

    const memoAPICore = axios.create({
        baseURL: 'http://localhost:8000/api/memos'
    });

    export default {
        name : 'MemoApp',
        data(){
            return{
                memos:[],
            };
        },
        components:{
            Memo,
            MemoForm,
            axios
        },
        methods:{
            // addMemo(payload){
            //     //밑에 꺼는 mapActions 헬퍼 함수의 addMemo 함수로 오버라이딩 되기 때문에 이 메소드는 실행되지 않는다.
            //     //어차피 오버라이드 되는데 같은 기능을 하니까 제거하라고 적혀있음.
            //     memoAPICore.post('/',payload)
            //     .then(res => {
            //         this.memos.push(res.data);
            //     });
            //     this.$emit('change',this.memos.length);
            //     // this.memos.push(payload);
            //     // this.storeMemo();
            // },
            storeMemo(){
                const memosToString = JSON.stringify(this.memos);
                localStorage.setItem('memos',memosToString);
            },
            deleteMemo(id){
                const targetIndex = this.memos.findIndex(v => v.id === id);

                memoAPICore.delete(`/${id}`)
                .then(() => {
                    this.memos.splice(targetIndex, 1);
                });
                this.$emit('change',this.memos.length);
                // this.memos.splice(targetIndex, 1);
                // this.storeMemo();
            },
            updateMemo(payload){
                const {id, content} = payload;
                const targetIndex = this.memos.findIndex(v => v.id === id);
                const targetMemo = this.memos[targetIndex];
                memoAPICore.put(`/${id}`,{content})
                .then(() => {
                    this.memos.splice(targetIndex, 1, {...targetMemo, content});
                });
                // this.memos.splice(targetIndex, 1, {...targetMemo, content});
                // this.storeMemo();
            },

            ...mapActions([
                'fetchMemos',
                'addMemo'
            ])
        },
    
    created(){
        // this.memo = localStorage.memos ? JSON.parse(localStorage.memos) : [];
        memoAPICore.get('/')
        .then(res =>{
            this.memos = res.data;
        });
    }
}

</script>

<style scoped>
.memo-list{
    padding: 20px 0;
    margin : 0;
}

</style>