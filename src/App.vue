<template>
  <BubbleSelectionInsertion />
</template>
<script>
import BubbleSelectionInsertion from "./components/BubbleSelectionInsertion.vue";
export default {
  name: "App",
  components: {
    BubbleSelectionInsertion,
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