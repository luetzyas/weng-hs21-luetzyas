<script>
import axios from "axios";
import {
    onMount
} from "svelte";
import BackButton from "../../reusable/backButton.svelte";

let menus = [];

onMount(() => {
    getMenus();
});

function getMenus() {
    axios
        .get("http://localhost:8080/website/menus")
        .then((response) => {
            menus = response.data;
        });
}

function deleteMenu(id, pos) {
    axios
        .delete("http://localhost:8080/website/menus/" + id)
        .then((response) => {
            alert("Menu " + menus[pos].label + " deleted");
            console.log(response.data);
        })
        .catch((error) => {
            console.log(error)
            alert(error)
        });
}
</script>

<div class="container">
    <h1>List of all available Menus</h1>

    <a href="#/create-menu" style="text-decoration: none;">
        <div class="d-grid gap-1">
            <button class="btn btn-outline-warning mb-3" type="button">
                + Add Menu
            </button>
        </div>
    </a>

    <BackButton />

    <div class="accordion according-flush mb-1" id="according">
        {#each menus as menu, pos}
        <div class="accordion-item">
            <h2 class="accordion-header" id="flush-heading{menu.id}">
                <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#collapse{menu.id}" aria-expanded="true" aria-controls="collapse{menu.id}">
                    <strong>Label: {menu.label}</strong>
                </button>
            </h2> <!-- end .accordion-header -->
            <div id="collapse{menu.id}" class="accordion-collapse collapse" aria-labelledby="flush-heading{menu.id}" data-bs-parent="#according">
                <div class="accordion-body">
                    <p><strong>layout: </strong>{menu.layout}</p>
                    <p><strong>This menu contains the following menus and items</strong></p>
                    <table class="table">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Navigation</th>
                                <th>Label</th>
                                <th>Views</th>
                            </tr>
                        </thead>
                        <tbody>
                            {#each menu.navigations as navigation}
                            <tr>
                                <td>
                                    {navigation.id}
                                </td>
                                {#if navigation.label != null}
                                <td>Menu</td>
                                <td>{navigation.label}</td>
                                <td>-</td>
                                {:else}
                                <td>Item</td>
                                <td>-</td>
                                <td>{navigation.ctrViews}</td>
                                {/if}
                            </tr>
                            {/each}
                        </tbody>
                    </table>
                    <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                        <button on:click={deleteMenu(menu.id, pos)} type="button" class="btn btn-outline-danger">
                            Delete Page
                        </button>
                    </div> <!-- d-grid gap-2 d-md-flex justify-content-md-end -->
                </div> <!-- end .accordion-body -->
            </div> <!-- end .accordion-collapse collapse -->
        </div> <!-- end .accordion-item -->
        {/each}
    </div> <!-- end .accordion according-flush mb-1 -->
</div> <!-- end .container -->
