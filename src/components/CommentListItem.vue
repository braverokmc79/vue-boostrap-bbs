<template>
 <div>
    <div class="comment-list-item">
      <div class="comment-list-item-name">
        <div>{{name}}</div>
        <div>댓글 번호: {{commentId}}</div>
        <div>{{commentObject.created_at}}</div>
      </div>
      <div class="comment-list-item-context">{{commentObject.context}}</div>
      <div class="comment-list-item-button">
        <b-button variant="info"  @click="modalCommentBtn(commentObject)">수정</b-button>
        <b-button variant="info" @click="commentDelete">삭제</b-button>
        <b-button variant="info" @click="subCommentToggle">덧글 달기</b-button>
      </div>
      
	</div>


  <template v-if="subCommentCreateToggle" >
    <SubCommentCreate class="mb-3" :commentId="commentId" :isSubComment="true"  :reloadSubComments="reloadSubComments"  :subCommentToggle="subCommentToggle"  />
  </template>  

  <template v-if="subCommentList.length >0">
      <div v-for="item in subCommentList" :key="item.subcomment_id" class="comment-list-item-subcomment-list ">                
          <div class="comment-list-item">
            <div class="comment-list-item-name">
              <div>{{item.user_name}}</div>
              <div>댓글 번호: {{item.comment_id}}</div>
              <div>서브 댓글 번호: {{item.subcomment_id}}</div>
              <div>{{item.created_at}}</div>
            </div>
            <div class="comment-list-item-context">{{item.context}}</div>
            <div class="comment-list-item-button">
              <b-button variant="info" @click="modalSubCommentBtn(item)" >수정</b-button>
              <b-button variant="info"  @click="commentSubDelete(item.subcomment_id)">삭제</b-button>            
            </div>
          </div>
      </div>
  </template>


  <div>
    <CommentUpdateModal  ref="commentUpdateModal"  :reloadComment="reloadComment" :commentObject="commentObject"  :commentIndex="commentIndex" />   
  </div>

 </div>	
</template>



<script>
import data from "@/data";
import SubCommentCreate from "@/components/CommentCreate";
import CommentUpdateModal from "@/Components/CommentUpdateModal";


export default {
	name:"CommentListItem",
	props:{
    commentObject:Object,
    reloadComment:Function,
    commentIndex:Number,
	},

  components:{
    SubCommentCreate,
    CommentUpdateModal
  },

	data(){

		return{
       commentId:this.commentObject.comment_id,
       modalCommentObject:this.commentObject,
			// userId:this.commentObject.user_id,
			// contentId:this.commentObject.content_id,
      // updatedAt:this.commentObject.updated_at,
      
      name:data.User.filter(item=> item.user_id===this.commentObject.user_id)[0].name,
      
      subCommentList:data.SubComment.filter(
        item=>item.comment_id===this.commentObject.comment_id
      ).map(subCommentItem =>{return {
        ...subCommentItem, user_name:data.User.filter(item=> item.user_id===subCommentItem.user_id)[0].name,
      }}),

     subCommentCreateToggle:false,

		}

  },
  
  methods:{
    subCommentToggle(){
      this.subCommentCreateToggle=!this.subCommentCreateToggle;
    },

    reloadSubComments(){
      this.subCommentList=data.SubComment.filter(
        item=>item.comment_id===this.commentObject.comment_id
      ).map(subCommentItem =>{return {
        ...subCommentItem, user_name:data.User.filter(item=> item.user_id===subCommentItem.user_id)[0].name,
      }})
    },

    commentDelete(){
      if(confirm("정말 삭제 하시겠습니까?")){
        
        let commentId=this.commentObject.comment_id;
        let findItem=data.Comment.find(function(item){ return item.comment_id===commentId});
        let idx=data.Comment.indexOf(findItem);
        
        console.log("findItem:");
        console.dir(findItem);       
        console.log("idx:"+idx);

        data.Comment.splice(idx, 1);               
        this.reloadComment();
      }
    },

    commentSubDelete(subcommentId){
       if(confirm("정말 삭제 하시겠습니까?")){ 
         let findItem=data.SubComment.find(function(item){ return item.subcomment_id==subcommentId});
         let idx=data.SubComment.indexOf(findItem);

         data.SubComment.splice(idx, 1);
         this.reloadSubComments();
      }
    },

    modalCommentBtn(commentObject){
      this.$refs.commentUpdateModal.$refs['my-modal'].hide();
      this.$refs.commentUpdateModal.$refs['my-modal'].show();
    },



  }



}
</script>

<style scoped>
.comment-list-item {
  display: flex;
  justify-content: space-between;
  padding-bottom: 1em;
}
.comment-list-item-name {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 0.5px solid black;
  padding: 1em;
}
.comment-list-item-context {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50em;
  border: 0.5px solid black;
}
.comment-list-item-button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 0.5px solid black;
  padding: 1em;
}
.btn {
  margin-bottom: 1em;
}
.comment-list-item-subcomment-list {
  display: flex;
  justify-content: space-between;
  padding-bottom: 1em;
  margin-left: 10em;
} 
</style>

