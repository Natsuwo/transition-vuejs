<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 offset-sm-2 col-md-6 offset-md-3 text-center">
        <h1>Animations</h1>
        <hr>
        <select v-model="typeAnimation" class="form-control mb-3">
          <option value="fade">Fade</option>
          <option value="slide">Slide</option>
        </select>
        <button class="btn btn-success" @click="show = !show">Show notification</button>
        <br>
        <br>
        <transition :name="typeAnimation">
          <div class="alert alert-success" v-show="show">Notification for transition</div>
        </transition>
        <transition :name="typeAnimation" mode="out-in">
          <div class="alert alert-success" v-if="show" key="success">Notification for transition</div>
          <div class="alert alert-primary" v-else key="primary">Notification for transition</div>
        </transition>
        <!-- <transition name="slide">
           <div class="alert alert-warning" v-if="show"> Notification for transition </div>
        </transition>
        <transition
         enter-class="" 
         enter-active-class="animated tada" 
         leave-class="" 
         leave-active-class="animated hinge" 
         appear>
           <div class="alert alert-danger" v-if="show"> Notification for transition </div>
        </transition>-->
        <hr>
        <button class="btn btn-success mb-3" @click="status = !status">Add or Remove</button>
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
          <div style="width: 300px; height: 100px; background: lightblue;" v-if="status"></div>
        </transition>
        <hr>
        <button
          class="btn btn-block btn-primary mb-3"
          @click="selectedComponent == 'appSuccessAlert' ? 
                selectedComponent = 'appDangerAlert' : 
                selectedComponent = 'appSuccessAlert' "
        >Submit (toogle 2 notfications)</button>
        <transition :name="typeAnimation" mode="out-in">
          <component :is="selectedComponent"></component>
        </transition>
        <hr>
        <button class="btn btn-block btn-warning mb-3" @click="addItem">Add Item</button>
        <ul class="list-group">
          <transition-group :name="typeAnimation">
            <li
              class="list-group-item"
              v-for="(number, index) in numbers"
              :key="number"
              style="cursor: pointer"
              @click="removeItem(index)"
            >{{ number }}</li>
          </transition-group>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import DangerAlert from "./components/DangerAlert";
import SuccessAlert from "./components/SuccessAlert";

export default {
  data() {
    return {
      show: true,
      status: false,
      elementWidth: 100,
      alertAniamtion: "fade",
      typeAnimation: "fade",
      selectedComponent: "appSuccessAlert",
      numbers: [1, 2, 3, 4, 5, 6]
    };
  },
  methods: {
    beforeEnter(el) {
      console.log("beforeEnter");
      this.elementWidth = 100;
      el.style.width = this.elementWidth + "px";
    },
    enter(el, done) {
      console.log("enter");
      let point = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth + point * 10 + "px";
        point++;
        if (point > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterEnter(el) {
      console.log("after enter");
    },
    enterCancelled(el) {
      console.log("enter Cancelled");
    },
    beforeLeave(el) {
      console.log("before Leave");
      this.elementWidth = 300;
      el.style.width = this.elementWidth + "px";
    },
    leave(el, done) {
      console.log("leave");
      let point = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth - point * 10 + "px";
        point++;
        if (point > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterLeave(el) {
      console.log("after leave");
    },
    leaveCancelled(el) {
      console.log("leave Cancelled");
    },
    addItem() {
      const pos = Math.floor(Math.random() * this.numbers.length);
      this.numbers.splice(pos, 0, this.numbers.length + 1);
    },
    removeItem(pos) {
      this.numbers.splice(pos, 1);
    }
  },
  components: {
    appDangerAlert: DangerAlert,
    appSuccessAlert: SuccessAlert
  }
};
</script>

<style>
/*enter */
.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity 0.5s;
}
/* leave */
.fade-leave {
  /* opacity: 1; */
}

.fade-leave-active {
  transition: opacity 0.5s;
  opacity: 0;
}
/* Slide transition effect*/

.slide-enter {
  opacity: 0;
}

.slide-enter-active {
  transition: opacity 0.5s;
  animation: slide-in 0.5s ease-out forwards;
}

.slide-leave-active {
  opacity: 0;
  transition: opacity 0.5s;
  animation: slide-out 0.5s ease-out forwards;
  position: absolute;
}

.slide-move {
  transition: transform 1s;
}

/* key frames */
@keyframes slide-in {
  from {
    transform: translateY(20px);
  }
  to {
    transform: translateY(0);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(20px);
  }
}
</style>
