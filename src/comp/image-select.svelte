<script>
    import {
        Appbar,
        Button,
        Grid,
        Paper,
        Screen,
    } from "svelte-doric"

    let screen = null
    let url = ""
    const close = () => screen.close(null)
    const use = () => screen.close(url)

    const input = document.createElement("input")
    input.type = "file"
    input.addEventListener(
        "change",
        (evt) => {
            const reader = new FileReader()
            reader.addEventListener(
                "load",
                () => url = reader.result
            )
            reader.readAsDataURL(input.files[0])
        }
    )

    const select = () => input.click()
</script>

<style>
    preview-area {
        position: relative;
        display: block;
        border: 1px solid var(--primary);
        border-radius: 4px;
        background-image: var(--bg);
        background-size: contain;
        background-position: center center;
        background-repeat: no-repeat;
    }
    preview-area::after {
        display: block;
        content: "";
        margin-top: 60%;
        width: 100%;
    }
    preview-text {
        position: absolute;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        user-select: none;
    }
</style>

<Screen bind:this={screen}>
    <Appbar slot="title">
        Image Select
    </Appbar>

    <div>
        <Paper card square>
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <preview-area on:click={select} style="--bg: url({url});">
                <preview-text>
                    Click to select image
                </preview-text>
            </preview-area>

            <Grid cols="1fr 1fr">
                <Button on:click={close} color="danger">
                    Cancel
                </Button>
                <Button on:click={use} color="secondary" disabled={url === ""}>
                    Use Image
                </Button>
            </Grid>
        </Paper>
    </div>
</Screen>
