<template>
  <div class="container">
    <div :class="['block', {animate: isAnimate}]"></div>
    <button @click="animateBlock">Animate</button>
  </div>
    <div class="container">
<!--        transition must only has one child inside-->
        <transition
            @before-enter="beforeEnter"
            @enter="enter"
            @after-enter="afterEnter"
            @enter-cancelled="enterCancelled"
            @before-leave="beforeLeave"
            @leave="leave"
            @after-leave="afterLeave"
            @leave-cancelled="leaveCancelled"
        >
            <p v-if="paraIsVisible">This is only sometimes visible...</p>
        </transition>
        <button @click="togglePara">Toggle para</button>
    </div>
    
    <div class="container">
        <transition name="toggle-users" mode="out-in">
            <button @click="showUsers" v-if="isUsersVisible">Show users</button>
            <button @click="hideUsers" v-else>Hide users</button>
        </transition>
    </div>
    
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
export default {
  data() {
    return { 
        dialogIsVisible: false,
        isAnimate: false,
        paraIsVisible: false,
        isUsersVisible: true,
        enterInterval: null,
        leaveInterval: null
    };
  },
  methods: {
    showUsers(){
        this.isUsersVisible = false
    },
    hideUsers(){
        this.isUsersVisible = true
    },
    showDialog() {
        this.dialogIsVisible = true;
    },
    hideDialog() {
        this.dialogIsVisible = false;
    },
    animateBlock() {
        this.isAnimate = true;
    },
    togglePara() {
        this.paraIsVisible = !this.paraIsVisible;
    },
    beforeEnter(el) {
        console.log('beforeEnter')
        console.log(el)
        el.style.opacity = 0;
    },
    enter(el, done) {
        console.log('enter')
        console.log(el)
        let round = 1;
        this.enterInterval = setInterval(()=>{
            el.style.opacity = round * 0.01;
            round ++;
            if(round > 100) {
                clearInterval(this.enterInterval);
                done();
            }
        },20)
    },
    afterEnter(el) {
        console.log('afterEnter')
        console.log(el)
    },
    enterCancelled(el) {
        console.log('enterCancelled')
        console.log(el)
        clearInterval(this.enterInterval)
    },
    beforeLeave(el) {
        console.log('beforeLeave')
        console.log(el)
        el.style.opacity = 1;
    },
    leave(el, done) {
        console.log('leave')
        console.log(el)
        let round = 1;
        this.leaveInterval = setInterval(()=>{
            el.style.opacity = 1 - round * 0.01;
            round++;
            if(round > 100) {
                clearInterval(this.leaveInterval)
                done();
            }
        },20)
    },
    afterLeave(el) {
        console.log('afterLeave')
        console.log(el)
    },
    leaveCancelled(el) {
        console.log('leaveCancelled')
        console.log(el)
        clearInterval(this.leaveInterval)
    },
    
    },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  transition: transform 0.3s ease-out;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.animate {
    /*transform: translateX(-150px);*/
    animation: slide-scale 0.3s ease-out forwards;/*forwards to keep the last state*/
}

/*different states (keyframes) in percentages or 'from' and 'to' words */
@keyframes slide-scale {
    0% {
        /*also change the size of the block (1 is the default value)*/
        transform: translateX(0) scale(1);
    }
    
    70% {
        transform: translateX(-120px) scale(1.1);
    }
    
    100% {
        transform: translateX(-150px) scale(1);
    }
}

/*Vue will add by default these three classes*/

.para-enter-from {
    opacity: 0;
    transform: translateY(-30px);
}

.para-enter-active {
    /*will keep track for all newly added css properties*/
    transition: all 0.3s ease-out;
}

.para-enter-to {
    opacity: 1;
    transform: translateY(0);/*real position it should have on this page*/
}

.para-leave-from {
    opacity: 1;
    transform: translateY(0);
}

.para-leave-active {
    transition: all 0.3s ease-in;
}

.para-leave-to {
    opacity: 0;
    transform: translateY(30px);
}

.toggle-users-enter-from,
.toggle-users-leave-to {
    opacity: 0;
}

.toggle-users-enter-active {
    transition: opacity 0.3s ease-out;
}

.toggle-users-leave-active {
    transition: opacity 0.3s ease-in;

}

.toggle-users-enter-to,
.toggle-users-leave-from {
    opacity: 1;    
}

</style>