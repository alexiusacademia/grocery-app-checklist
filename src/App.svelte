<script>
  import { fly } from "svelte/transition";

  let groceryItems = [];
  let newItem = "";

  function addItem() {
    if (newItem.trim() !== "") {
      groceryItems = [
        ...groceryItems,
        { id: Date.now(), text: newItem, done: false },
      ];
      newItem = "";
    }
  }

  async function removeItem(id) {
    const itemToRemove = groceryItems.find((item) => item.id === id);
    itemToRemove.done = true;
    await new Promise((resolve) => setTimeout(resolve, 500));
    groceryItems = groceryItems.filter((item) => item.id !== id);
  }
</script>

<div class="container">
  <h1>Grocery Checklist</h1>

  <div style="text-align: center;">
    <input
      type="text"
      bind:value={newItem}
      placeholder="Enter a grocery item"
      on:keydown={(e) => (e.key === "Enter" ? addItem() : "")}
    />
    <button on:click={addItem}>Add Item</button>
  </div>

  <ul>
    {#each groceryItems as item (item.id)}
      <li>
        <input type="checkbox" on:change={() => removeItem(item.id)} />
        <span
          class={item.done ? "done" : ""}
          transition:fly={{ y: 20, duration: 500 }}>{item.text}</span
        >
      </li>
    {/each}
  </ul>
</div>

<style>
  .container {
    max-width: 600px;
    margin: 0 auto;
    font-family: "Arial", sans-serif;
  }
  h1 {
    text-align: center;
  }
  input,
  button {
    font-size: 1rem;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
  }
  .done {
    text-decoration: line-through;
  }
</style>
