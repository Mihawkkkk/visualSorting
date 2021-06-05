<template>
  <BubbleSelection />
</template>
<script>
import BubbleSelection from "./components/bubbleSelection";
export default {
  name: "App",
  components: {
    BubbleSelection,
  },
};
</script>

<style >
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Signika Negative", sans-serif;
  text-transform: capitalize;
}
h1 {
  text-align: center;
}
</style>