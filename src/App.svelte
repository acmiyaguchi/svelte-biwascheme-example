<script>
  import BiwaScheme from "biwascheme";
  import CodeMirror from "codemirror/lib/codemirror.js";
  import "codemirror/mode/scheme/scheme.js";
  import "codemirror/lib/codemirror.css";

  let errorMessage;
  let biwascheme = new window.BiwaScheme.Interpreter(
    e => (errorMessage = e.message)
  );

  let editorElement;
  let editor;
  $: editor = editorElement
    ? CodeMirror(editorElement, {
        value: `(print "hello world")`,
        lineNumbers: true,
        mode: "text/x-scheme"
      })
    : null;
</script>

<style>
  main {
    max-width: 900px;
    margin: 0 auto;
  }

  .error {
    border: 1px solid red;
  }
</style>

<main>
  <h1>biwascheme repl</h1>

  <p>
    This is an example of using
    <a href="https://www.biwascheme.org/">biwascheme</a>
    with a bundler like rollup. See the
    <a href="https://github.com/acmiyaguchi/svelte-biwascheme-example">
      source code on Github
    </a>
    .
  </p>

  <div style="border:1px solid" bind:this={editorElement} />
  <div>
    <button
      on:click={() => {
        errorMessage = null;
        biwascheme.evaluate(editor.getValue());
      }}>
      Run
    </button>
  </div>

  {#if errorMessage}
    <div class="error">{errorMessage}</div>
  {/if}

  <div id="bs-console" />
</main>
