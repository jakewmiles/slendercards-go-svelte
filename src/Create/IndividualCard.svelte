<script>
  import { fade, fly } from "svelte/transition";
  export let visible, example, srcEmoji, srcLang, targEmoji, targLang;
  let examplePosted = false;

  const postSentence = async () => {
    if (examplePosted) {
      console.log("Sentence already posted to database!!");
      return;
    }
    examplePosted = true;
    return await fetch("http://localhost:3000/flashcards", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        srcLang,
        targLang,
        srcSentence: example.SourceSentence,
        targSentence: example.TargetSentence,
      }),
    });
  };
</script>

{#if visible}
  <div class="example-card" transition:fade>
    <p class="sentence">{srcEmoji} {example.SourceSentence}</p>
    <p class="sentence">{targEmoji} {example.TargetSentence}</p>
    <button
      class="card-selector"
      on:click={() => {
        visible = false;
        postSentence();
      }}>✅</button
    >
  </div>
{/if}

<style>
  .example-card {
    width: 100%;
    align-items: center;
    display: flex;
    justify-content: center;
    padding: 0;
  }

  .sentence {
    margin: 10px;
    width: 100%;
    height: 100%;
    padding: 5px;
    overflow: scroll;
    box-shadow: 0 15px 30px 0 rgba(109, 109, 109, 0.208);
    background-color: rgb(30, 30, 30);
    border-radius: 0.5rem;
    border-left: 0 solid #ff6600;
    transition: border-left 150ms ease-in-out, padding-left 150ms ease-in-out;
  }

  .sentence:hover {
    padding-left: 0.5rem;
    border-left: 0.5rem solid #ff6600;
  }

  .card-selector {
    background-color: transparent;
    border: none;
    text-align: center;
    font-size: 30px;
    cursor: pointer;
  }
</style>
