<template>
	
<b-container fluid class="mt-2 p-5">
  <b-row class="mt-2">
	  <b-col sm="12" >
		<b-input v-model="subject" placeholder="제목을 입력해 주세요" ></b-input>
	  </b-col>
	  <b-col sm="12" class="mt-2">
		<b-form-textarea
			v-model="context"
			placeholder="내용을 입력해 주세요"
			rows="3"
			max-rows="10"
		></b-form-textarea>
	   </b-col>
	 

	<b-col sm="12" class="mt-2">	
		<b-button @click="updateMode?  updateContent():uploadContent() ">저장</b-button>
		<b-button @click="cancle">취소</b-button>
	 </b-col>
	</b-row>
	</b-container>
		
</template>

<script>
import data from  '@/data';

export default {
	name:"Create",
	data(){

		return {
			now: "00:00:00",
			subject:"",
			context:"",
			userId:1,
			createdAt:"",
			updatedAt:null,
			updateObject:null,
			updateMode:this.$route.params.contentId>0?true:false
		}
	},

	created(){
		if(this.$route.params.contentId>0){
			const contentId=Number(this.$route.params.contentId);
			this.updateObject=data.Content.filter(item=>item.content_id===contentId)[0];
			this.subject=this.updateObject.title;
			this.context=this.updateObject.context;
			this.createdAt=this.updateObject.created_at;
		}
	},

	methods:{

		uploadContent(){

				let items=data.Content.sort((a,b)=>{return b.content_id-a.content_id});
				const content_id=items[0].content_id+1;
				data.Content.push({
					    content_id: content_id,
						user_id: this.userId,
						title: this.subject,
						context: this.context,
						created_at: this.currentTime(),
						updated_at: null
				});

				this.$router.push({
						path:'/board/free/'
				});
		},



		updateContent(){
			this.updateObject.title=this.subject;
			this.updateObject.context=this.context;
			this.updateObject.updated_at=this.currentTime();
			this.$router.push({
				 path:'/board/free/'
			});
		},


		cancle(){
			this.$router.push({
				path:'/board/free/'
			});
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