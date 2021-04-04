<template>
  <div>		
	  <div  v-for="item in comments" :key="item.comment_id"> 
	  	<div>
			  <CommentListItem  :commentObject="item"  :reloadComment="reloadComment" />
		</div>
	  </div>

	<div class="mb-5">
		<CommentCreate :contentId="contentId" :reloadComment="reloadComment" />
	 </div>	  
  </div>  
</template>

<script>
import data from  '@/data';
import CommentListItem from "@/components/CommentListItem";
import CommentCreate from "@/components/CommentCreate";

export default {
	name:"CommentList",
	props:{
		contentId:Number,
	},

	components:{
		CommentListItem,
		CommentCreate
	},
	data(){
		return {
			comments:data.Comment.filter(item => item.content_id === this.contentId)
		}		
	},


	methods:{
		reloadComment(){
			this.comments=data.Comment.filter(item => item.content_id === this.contentId)
		}
	}


}
</script>

<style>

</style>