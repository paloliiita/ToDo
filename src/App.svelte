<script>
    import { Collection } from "japidb";
    import { onMount } from "svelte";
    const task = new Collection("Task", "id");
    let ToDoText = "";
    let items = [];
    onMount(() => {
        items = task.find();
    });

    function CheckTask(item) {
        item.done = !item.done;
        task.save(item);
        items = task.find();
    }

    function AddToToDo() {
        const ToDo = {
            id: items.length + 1,
            text: ToDoText,
            done: false,
        };
        task.save(ToDo);

        items = [...items, ToDo];
        ToDoText = "";
    }
</script>

<main>
    <div id="formulario">
        <h1>To do list...</h1>
        <div class="input-group mb-3">
            <input
                type="text"
                placeholder="What to do"
                class="form-control"
                bind:value={ToDoText}
            />
            <button
                on:click={AddToToDo}
                type="button"
                class="btn btn-outline-secondary">+</button
            >
        </div>
    </div>

    <div id="body">
        <div id="lista">
            <h3>To do</h3>
            <ul class="list-group">
                {#each items as item}
                    {#if !item.done}
                        <li class="list-group-item">
                            <input
                                type="checkbox"
                                on:click={(_) => CheckTask(item)}
                                checked={item.done}
                            />
                            {item.text}
                        </li>
                    {/if}
                {/each}
            </ul>
        </div>
        <br />
        <br />
        <div id="lista">
            <h3>Done</h3>
            <ul class="list-group">
                {#each items as item}
                    {#if item.done}
                        <li class="list-group-item">
                            <input
                                type="checkbox"
                                on:click={(_) => CheckTask(item)}
                                checked={item.done}
                            />
                            {item.text}
                        </li>
                    {/if}
                {/each}
            </ul>
        </div>
    </div>

</main>

<style>
    div#formulario {
        background-color: #f8bbd0;
        padding: 10px;
        font-family: Comic Sans MS, Comic Sans, cursive;
        color: #880e4f
    }
    :global(body) {
        margin: 0;
        padding: 0px;
    }

    div#body {
        background-color: #fbe9e7;
        height: 100em;
        padding: 10px;
        font-family: Comic Sans MS, Comic Sans, cursive;
        color: #ff5c8d
    
        
    }
</style>
