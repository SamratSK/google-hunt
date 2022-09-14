<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import Loader from "./Loader.svelte";
  import { error } from "./stores/common";

  let question: string = "Loading...";
  let loaderVisible: boolean = false;
  let unsubscriber = error.subscribe((value) => {
    if (value) {
      document.body.style.backgroundColor = "rgb(44, 1, 2)";
      return;
    }

    document.body.style.backgroundColor = "rgb(1, 18, 44)";
  });

  onMount(() => {
    let [_, batch, slug] = window.location.hash.split("/");

    loaderVisible = true;
    console.log(loaderVisible);
    fetch(`http://googlehunt.maginnow.com/api/questions/${batch}/${slug}`)
      .then((res) => res.json())
      .then((data: [any]) => {
        if (data.length <= 0) {
          question = "Unable to retrieve question!";
          return;
        }

        error.set(false);
        question = data[0].text;
      })
      .finally(() => {
        loaderVisible = false;
      });
  });

  onDestroy(() => {
    unsubscriber();
  });
</script>

<main>
  <header class="cursive">Google Hunt</header>
  <div class="question">{question}</div>

  {#if loaderVisible}
    <Loader />
  {/if}
</main>

<style lang="scss" scoped>
  main {
    width: 90%;

    display: flex;
    flex-direction: column;

    align-items: center;
    justify-content: center;
  }

  header {
    margin: 0;
    font-size: 4rem;
  }

  .question {
    margin-top: 1.5rem;
    font-size: 105%;
  }
</style>
