<template>
  <div>
    <b-modal
      id="commentUpdate-modal"
      ref="my-modal"
      title="수정 하기"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-input
            ref="modalCommentId"
            v-model="commentId"
            :state="commentIdState"
            required
            hidden
          ></b-form-input>

          <b-form-textarea
            ref="modalContext"
            v-model="context"
            :state="contextState"
            required
          ></b-form-textarea>

      </form>
    </b-modal>


    <b-modal
      id="commentSubUpdate-modal"
      ref="my-sub-modal"
      title="서브 댓글 수정 하기"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubSubmit">     
          <b-form-textarea
            ref="subContext"
            id="sub_context"
            v-model="context"          
            required
          ></b-form-textarea>
      </form>
    </b-modal> 

  </div>
</template>


<script>
import data from "@/data";


 export default {
    name:"CommentUpdateModal",

    props:{
       reloadComment:Function,     
       commentObject:Object, 
       commentIndex:Number  ,
       reloadSubComments:Function 
    },


    data() {
      return {
        context: this.commentObject.context,
        commentId:this.commentObject.comment_id,
        contextState: null,
        commentIdState:null,
      }

    },

  
     methods: {
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.contextState = valid
        return valid
      },
      resetModal() {
       
      },
      handleOk(bvModalEvt) {
        // Prevent modal from closing
        bvModalEvt.preventDefault()
        // Trigger submit handler
        this.handleSubmit()
      },

      handleSubmit() {
        // Exit when the form isn't valid
        if (!this.checkFormValidity()) {
          alert("댓글을 작성해 주세요.");          
          this.$refs.modalContext.focus;
          return
        }

        //데이타 업데이트                
        let commentId=this.commentId;
        data.Comment.filter(item=>item.comment_id===commentId)[0].context=this.context;
        this.reloadComment();


        //모달 닫
        this.$nextTick(() => {
          this.$bvModal.hide('commentUpdate-modal');
        });    
      },


      handleSubSubmit() {
        // Exit when the form isn't valid
        if (!this.checkFormValidity()) {
          alert("댓글을 작성해 주세요.");          
          this.$refs.modalContext.focus;
          return
        }


      },


    }
  }
</script>