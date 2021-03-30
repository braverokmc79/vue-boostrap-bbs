<template>
<b-container fluid class="mt-5 p-5">
  <b-row class="mt-2">
    <b-col sm="2">
      <label for="textarea-small">글번호:</label>
    </b-col>
    <b-col sm="10">
       <b-form-input v-model="contentId" readonly ></b-form-input>
    </b-col>
  </b-row>

  <b-row class="mt-2">
    <b-col sm="2">
      <label for="textarea-small">글쓴이:</label>
    </b-col>
    <b-col sm="10">
       <b-form-input v-model="user" readonly ></b-form-input>
    </b-col>
  </b-row>

  <b-row class="mt-2">
    <b-col sm="2">
      <label for="textarea-default">등록일:</label>
    </b-col>
    <b-col sm="10">
      <b-form-input v-model="created" readonly ></b-form-input>
    </b-col>
  </b-row>

  <b-row class="mt-2">
    <b-col sm="2">
      <label for="textarea-large">내용:</label>
    </b-col>
    <b-col sm="10">
      <b-form-textarea      
        size="lg"
        rows="10"
        
      ></b-form-textarea>
    </b-col>
  </b-row>


  <b-row class="mt-2">
      <b-col sm="12" class="text-center">
         <b-button variant="primary" @click="updateData">수정</b-button>
         <b-button variant="success" @click="deleteData">삭제</b-button>
      </b-col>
  </b-row> 

<b-row class="mt-5 mb-5 comment">
    <b-col sm="12">
      <span>댓글</span>
    </b-col>   
  </b-row>

</b-container>
</template>



<script>
import data from '@/data';
export default{

  name:"ContentDetail",

  data(){
    const contentId=Number(this.$route.params.contentId);
    const contentData=data.Content.filter(item=>item.content_id===contentId)[0]
    return{
      contentId:contentId,
      title:contentData.title,
      context:contentData.context,
      user:data.User.filter(item=>item.user_id===contentData.user_id)[0].name,
      created:contentData.created_at
    }
  },

  methods:{
      updateData(){

      },

      deleteData(){
         const content_index = data.Content.findIndex(item => item.content_id === this.contentId);
          data.Content.splice(content_index, 1)
          this.$router.push({
            path: '/board/free'
          });
      }


  }

}
</script>
<style scoped>
.comment{
  border: 1px solid black;
  margin-top: 1rem;
  padding: 2rem;
}
</style>
