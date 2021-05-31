<template>
  <h1>{{ msg }}</h1>
  <div class="times">
    <!-- <p>{{ timeStamp }}</p> -->
    <!-- <p>{{ localTime }}</p> -->

    <p>{{ Date.now() }}</p>
    <button @click="count++">点击: {{ count }}次</button>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  name: "HelloWorld",
  props: {
    msg: {
      type: String,
      required: true,
    },
    data() {
      return {
        timeStamp: 123,
      };
    },
  },
  setup: () => {
    const count = ref(0);
    let timeStamp = ref();
    let localTime = ref(new Date().toLocaleTimeString());
    console.log("setup: ", timeStamp);

    axios
      .get("http://quan.suning.com/getSysTime.do")
      .then(timeOk)
      .catch((error) => console.log(error));

    function timeOk(response) {
      timeStamp.value = response.data.sysTime2.split(" ")[1];
      console.log(response.data.sysTime2);
      console.log(timeStamp);
    }
    /* setInterval(() => {
      console.log(timeStamp);
      // localTime.value = Date.now()
      localTime.value = new Date().toLocaleTimeString()
    }, 1000); */

    return { count, timeStamp, localTime };
  },
});
</script>

<style scoped lang="scss">
.times {
  text-align: center;
  font-size: 30px;
  button {
    color: red;
  }
}
</style>
