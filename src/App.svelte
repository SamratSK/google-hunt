<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { error } from "./stores/common";

  let question: string;
  let unsubscriber = error.subscribe((value) => {
    if (value) {
      document.body.style.backgroundColor = "rgb(44, 1, 2)";
      return;
    }

    document.body.style.backgroundColor = "rgb(1, 18, 44)";
  });

  onMount(() => {
    let [_, batch, slug] = window.location.pathname.split("/");
    fetch(`http://googlehunt.maginnow.com/api/questions/${batch}/${slug}`)
      .then((res) => res.json())
      .then((data: [any]) => {
        if (data.length <= 0) {
          error.set(true);
          question = "Unable to retrieve question!";
        }

        error.set(false);
        question = data[0].text;
      });
  });

  onDestroy(() => {
    unsubscriber();
  });
</script>

<main>
  <header class="cursive">Google Hunt</header>
  <div class="url">www.url.com/&lt;your answer&gt;</div>
  <div class="question">{question}</div>
</main>

<style lang="scss">
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

  .url {
    margin-top: 0.75rem;
  }

  .question {
    margin-top: 1.5rem;
    font-size: 105%;
  }
</style>
