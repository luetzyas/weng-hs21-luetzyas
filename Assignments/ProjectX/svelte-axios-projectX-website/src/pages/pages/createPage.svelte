<script>
import axios from "axios";

let page = {
    language: "",
    name: null,
};

let languages = ["", "Englisch", "Französisch", "Portugisisch", "Spanisch", "Russisch", "Deutsch", "Ungarisch", "Italienisch", "Dänisch", "Bulgarisch"];

function addPage() {
    axios
        .post("http://localhost:8080/website/pages/", page)
        .then((response) => {
            alert("Page " + page.name + " added");
            page.language = "";
            page.name = null;
            console.log(response.data);
        })
        .catch((error) => {
            console.log(error)
            alert(error)
        });
}
</script>

<div class="container">
    <div class="mb-5">
        <h1 class="mt-3">Add a Page</h1>

        <form>
            <div class="mb-3">
                <label for="" class="form-label">Name</label>
                <input
                    class="form-control"
                    type="text"
                    bind:value={page.name}
                    />
            </div> <!-- end .mb-3 -->
            <div class="mb-3">
                <label for="" class="form-label">Language</label>
                <select bind:value={page.language} class="form-select">
                    {#each languages as language}
                    <option>{language}</option>
                    {/each}
                </select>
            </div><!-- end .mb-3 -->
            <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                <button on:click={addPage} type="button" class="btn btn-warning">
                    Add Page
                </button>
                <a href="#/page">
                    <button type="button" class="btn btn-outline-warning">
                        Back to Pageoverview
                    </button>
                </a>
            </div> <!-- end .d-grid gap-2 d-md-flex justify-content-md-end -->
        </form>
    </div> <!-- end .mb-5 -->
</div> <!-- end .container -->
