<script>
  import { onMount, onDestroy, createEventDispatcher } from "svelte";

  export let startTimer;

  export let time = {
    s: 0,
    m: 0,
    h: 0
  };

  function setTimer() {
    time.s++;
    if (time.s === 60) {
      time.s = 0;
      time.m++;
      if (time.m === 60) {
        time.m = 0;
        time.h++;
        if (time.h === 24) {
          time = {
            s: 0,
            m: 0,
            h: 0
          };
          alert("damn it you took a long to solve this game");
        }
      }
    }
    time = time;
  }
  let t;
  onMount(() => {
    if (startTimer) {
      startTimer = false;
      t = setInterval(setTimer, 1000);
    }
  });

  const dispatch = createEventDispatcher();
  
  onDestroy(() => {
    clearInterval(t);
     dispatch("time", {
      time
    });
  });
</script>

<style>

</style>

<div>{time.h} : {time.m} : {time.s}</div>
