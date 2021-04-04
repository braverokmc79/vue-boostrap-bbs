<template>
  <div>
	<b-input-group :prepend="name" class="mt-3">
    <b-form-textarea
      v-model="context"
      placeholder="코멘트를 달아 주세"
      rows="3"
      max-rows="6"
    ></b-form-textarea>
	
	<b-input-group-append>
		<b-button variant="info" @click="isSubComment ?   createSubComment():createComment()">작성하기</b-button>	
	</b-input-group-append>
	
	</b-input-group>
  </div>	  
</template>

<script>
import data from  "@/data";

export default {
  name:"CommentCreate",
  props:{
    contentId:Number,
    reloadComment:Function,
    commentId:Number,
    isSubComment:Boolean,
    reloadSubComments:Function,
    subCommentToggle:Function
  },

	data(){
		return {
			name:"르라나",   
      context:''
		}
  },
  
  methods:{
    createComment(){
       data.Comment.push(
        {
          comment_id:data.Comment[data.Comment.length-1].comment_id+1,
          user_id:1,
          content_id:this.contentId,
          context:this.context,
          created_at:this.currentTime(),
          updated_at:null
        });
        this.reloadComment();
        this.context="";
    },

    createSubComment(){
       
        let subCommentobj={
          subcomment_id:data.SubComment[data.SubComment.length-1].subcomment_id+1,
          user_id:1,
          comment_id:this.commentId,
          context:this.context,
          created_at:this.currentTime(),
          updated_at:null
        }

        data.SubComment.push(
          subCommentobj
        );
        this.reloadSubComments();
        this.context="";
        this.subCommentToggle();
    },

    currentTime(){
        let today = new Date();  
        let year = today.getFullYear(); // 년도
        let month = today.getMonth() + 1;  // 월
        let date = today.getDate();  // 날짜
        let day = today.getDay();  // 요일
        
        month=this.dateFormat(month);
        date=this.dateFormat(date);		
        let currentDay=year + '-' + month + '-' + date;

        this.now = " "+this.dateFormat(today.getHours()) + ":" + this.dateFormat(today.getMinutes()) + ":" +this.dateFormat(today.getSeconds());
        console.log(currentDay+this.now);
        return 	currentDay+this.now;		
    },

    dateFormat(str){
      str= "00"+str;
      return str=str.slice(-2);
    }

  }
	

}


</script>

<style>

</style>