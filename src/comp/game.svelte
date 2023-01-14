<script>
    import {Toggle} from "svelte-doric"

    export let pieceArray
    export let move
    export let url

    let showNums = true
    const vars = (pos) =>
        `--x: ${pos.x}; --y: ${pos.y};`
    const offset = (value) => {
        const n = parseInt(value) - 1
        const x = n % 4
        const y = Math.floor(n / 4)
        return `--ox: ${x * -100}%; --oy: ${y * -100}%;`
    }
</script>

<style>
    game-board {
        position: relative;
        display: block;
        transition: width 100ms linear;
    }
    game-board::after {
        display: block;
        content: "";
        margin-top: 100%;
        width: 100%;
    }

    game-piece {
        position: absolute;
        display: grid;
        padding: 2px;
        width: 25%;
        height: 25%;
        top: 0px;
        left: 0px;
        transform: translate(
            calc(var(--x) * 100%),
            calc(var(--y) * 100%)
        );
        transition: transform 100ms linear;
    }
    game-piece-content {
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 32px;
        border-radius: 4px;
        border: 3px solid var(--primary);
        overflow: hidden;
    }
    game-piece-content::after {
        content: "";
        position: absolute;
        top: var(--oy);
        left: var(--ox);
        width: 400%;
        height: 400%;
        z-index: -1;
        background-image: var(--bg);
        background-repeat: no-repeat;
        background-size: 100% 100%;
    }
    span {
        background-color: var(--card-background);
    }
</style>

{#if url !== ""}
    <Toggle checkbox bind:on={showNums} label="Show piece numbers" />
{/if}
<game-board style="--bg: url({url});">
    {#each pieceArray as [value, pos] (value)}
        {#if value !== "0"}
            <game-piece style={vars(pos)} on:pointerdown={move(value)}>
                <game-piece-content style={offset(value)}>
                    {#if showNums}
                        <span>{value}</span>
                    {/if}
                </game-piece-content>
            </game-piece>
        {/if}
    {/each}
</game-board>
