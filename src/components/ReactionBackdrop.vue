<template>
  <div v-if="showModal" class="modal">
    <div class="modal-content" @click="closeModal">
      <h1>Click Me</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "ReactionBackdrop",
  data() {
    return {
      showModal: false,
      timer: null,
      reactionTime: 0
    };
  },
  props : {

  },
  methods: {
    openModal() {
      this.showModal = true;
      this.reactionTime = 0;
      this.startTimer();
    },
    closeModal() {
      this.showModal = false;
      clearInterval(this.timer);
      this.$emit('reaction-time', this.reactionTime);
    },
    startTimer() {
      this.timer = setInterval(()=> {
        this.reactionTime += 10;
      });
    }
  }
}
</script>

<style>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0,0,0,0.4);
  display: block;
}

.modal-content {
  border-radius: 20px;
  background-color: rgba(255,255,255, 0.8);
  backdrop-filter: blur( 2px );
  -webkit-backdrop-filter: blur( 2px );
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  color: #FF204E;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Dubai Medium";
}

</style>