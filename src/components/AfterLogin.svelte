<script>
    import { navigate } from "svelte-navigator";
    let userType;
    $: {
        console.log(userType)
    }
</script>

<form
    on:submit|preventDefault={(_) => {
        userType = userType.checked ? "B" : "U";
        localStorage.setItem("userType", userType);
        navigate("/");
    }}
>
    <div>
        <label for="userType">
            <input type="checkbox" bind:this={userType} id="userType" /> I want Land details
        </label>
        <br>
        <br>
        <button type="submit">Proceed</button>
    </div>
</form>

<style>
    form {
        position: absolute;
        inset: 0;
        display: grid;
        place-items: center;
        text-align: center;
    }
    button {
        padding: 0.5em 1em;
        font-size: 1em;
        border-radius: 0.5em;
        border: 0;
        background-color: #00bcd4;
    }
</style>
