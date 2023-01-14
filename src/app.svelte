<script context="module">
    const handler = (func) =>
        (...args) =>
            (evt) => func(evt, ...args)
</script>

<script>
    import {
        AppStyle,
        Baseline as baseline,
        TronTheme as theme,

        Appbar,
        Button,
        Paper,
        Screen,
    } from "svelte-doric"

    import game from "./game.mjs"

    import Game from "./comp/game.svelte"
    import ImageSelect from "./comp/image-select.svelte"
    import Timer from "./comp/timer.svelte"

    let screen = null
    let url = ""
    const selectImage = handler(
        async () => {
            const image = await screen.openStack(ImageSelect)

            if (image === null) {
                return
            }

            url = image
        }
    )

    let timer = null
    let playing = false
    let pieces = game.solved
    $: pieceArray = Object.entries(pieces)

    const newGame = handler(
        () => {
            pieces = game.scramble()
            playing = true
            timer.start()
        }
    )

    const manhattanDist = (a, b) => (
        Math.abs(a.x - b.x)
        + Math.abs(a.y - b.y)
    )
    const move = handler(
        (evt, value) => {
            if (playing === false) {
                return
            }
            const pos = pieces[value]
            const pos0 = pieces["0"]
            const dist = manhattanDist(pos, pos0)
            if (dist !== 1) {
                return
            }
            pieces = {
                ...pieces,
                [value]: pos0,
                "0": pos,
            }
        }
    )
    $: if (playing === true && game.isSolved(pieces) === true) {
        playing = false
        timer.stop()
    }
</script>

<AppStyle {baseline} {theme} />

<Screen bind:this={screen}>
    <Appbar slot="title">
        15 Puzzle

        <Button on:click={newGame()} adorn slot="action">
            New Game
        </Button>
    </Appbar>

    <Paper card square>
        <Button on:click={selectImage()} variant="outline" color="secondary">
            Set Custom Image
        </Button>

        <Game {pieceArray} {move} {url} />

        <Timer bind:this={timer} />
    </Paper>
</Screen>
