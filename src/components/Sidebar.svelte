<script>
    import img1 from "../assets/1.jpg"
    import img2 from "../assets/2.webp"
    import img3 from "../assets/3.jpg"


    export let showSidebar = false;

    export let details = {};

    const userType = localStorage.getItem("userType");


    let up = 0 , down = 0;
</script>

{#if showSidebar}
    <div id="sidebar">
        <button id="close" on:click={(_) => (showSidebar = false)}>×</button>
        {#if userType == "B" || details.name == "Canditate Point"}
            <div class="landDetails">
                <b>{details.name}</b>
                <br>
                <br>
                <p><b>Owner:</b> Jenin</p>
                {#if userType == "B"}
                    <p><b>Estimated worth:</b> 100k</p>
                {:else}
                    <div>
                        <button on:click={_=>up++}>⬆️ {up}</button>
                        <!-- <button on:click={_=>down++}>👎 {down}</button> -->
                    </div>
                {/if}
            </div>
        {:else}
            <div class="registerDetails">
                <b>{details.name}</b>
                <br>
                <br>
                <div class="image-grid">
                    <img src={img1} alt="" />
                    <img src={img2} alt="" />
                    <img src={img3} alt="" />
                </div>
                <p><b>Provider: </b>{details.provider}</p>
                <p><b>Time to reach: </b>30mins</p>
                <button>Register</button>
            </div>
        {/if}
    </div>
{/if}

<style>
    #sidebar {
        font-family: sans-serif;
        position: absolute;
        inset: 2em 3em 2em auto;
        width: 30vw;
        background-color: #fff;
        border-radius: 1em;
        box-shadow: 0 0.5em 1em 0 rgba(0, 0, 0, 0.2);
        z-index: 1;
        padding: 1em;
        display: grid;
        place-items: center;
        text-align: center;
    }

    #close {
        position: absolute;
        right: 0;
        top: 0;
        border: 0;
        border-radius: 0 3px 0 0;
        cursor: pointer;
        background-color: transparent;
        font-size: 2em;
        color: black;
    }

    .image-grid {
        display: flex;
        gap: 5px;
        flex-wrap: wrap;
        justify-content: center;
    }

    .image-grid img {
        max-width: min(calc(30vw - 2em), 200px);
        object-fit: cover;
        border-radius: 0.5em;
    }

    button {
        border: 0;
        border-radius: 0.5em;
        padding: 0.5em 1em;
        background-color: transparent;
        /* color: #fff; */
        font-size: 1em;
        cursor: pointer;
    }
</style>
