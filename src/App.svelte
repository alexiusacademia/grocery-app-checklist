<script>
  const keyName = "items";

  let items = [];

  if (localStorage.getItem(keyName) === null) {
    localStorage.setItem(keyName, JSON.stringify(items));
  }

  items = JSON.parse(localStorage.getItem(keyName));

  let name = "";

  const updateLocalStorage = () => {
    localStorage.setItem(keyName, JSON.stringify(items));
  };

  const addItem = () => {
    items = [
      ...items,
      {
        id: Math.random(),
        name,
        done: false,
      },
    ];

    updateLocalStorage();

    name = "";
  };

  const remove = (item) => {
    items = items.filter((i) => i !== item);
  };

  const toggle = (item) => {
    updateLocalStorage();
  };
</script>

<div>
  <h1>Grocery List 🗒️</h1>

  <form on:submit|preventDefault={addItem}>
    <label for="name">Add an item</label>
    <input id="name" type="text" bind:value={name} />
  </form>

  <ul>
    {#each items as item}
      <li class:done={item.done}>
        <input
          type="checkbox"
          bind:checked={item.done}
          on:change={() => toggle(item)}
        />
        <span>{item.name}</span>
        <button on:click={() => remove(item)}>&times;</button>
      </li>
    {/each}
  </ul>
</div>

<style>
  div,
  h1 {
    color: #333;
    max-width: 300px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
      Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
  }
  #name {
    width: 100%;
  }
  form {
    margin-bottom: 0.5em;
  }
  input[type="text"] {
    outline: none;
    margin: 0;
  }
  input[type="text"]:focus {
    border-color: #dc4f21;
    box-shadow: 0 0 2px #dc4f21;
  }
  input[type="checkbox"] {
    margin: 0 10px 0 0;
  }
  li button {
    float: right;
    border: none;
    background: transparent;
    padding: 0;
    margin: 0;
    color: #dc4f21;
    font-size: 18px;
    cursor: pointer;
  }
  li button:hover {
    transform: scale(2);
  }
  li button:focus {
    outline: #dc4f21;
  }
  li:last-child {
    border-bottom: none;
  }
  label {
    display: block;
    text-transform: uppercase;
    font-size: 0.8em;
    color: #777;
  }
  li {
    list-style: none;
    padding: 6px 10px;
    border-bottom: 1px solid #ddd;
  }
  ul {
    padding-left: 0;
  }
  .done span {
    opacity: 0.4;
  }
</style>
