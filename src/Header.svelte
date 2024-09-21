<script>
    let isOpen = false;

    import InfoIcon from "./InfoIcon.svelte";

    function toggleDrawer() {
        isOpen = !isOpen;
    }
</script>

<div>
    <div id="title" class="flex mt-4 bg-grey">
        <button on:click={toggleDrawer}>
            <div>
                <div class="space">&nbsp;</div>
                <h1>Bernklang</h1>
                <div class="info-icon">
                    <InfoIcon />
                </div>
            </div>
        </button>

        <main class="info" class:is-open={isOpen}>
            <div>The sounds of Berne, Switzerland</div>

            <div class="contrib">
                <a href="mailto:stefan.heinemann@codedump.ch">by Stefan Heinemann</a><br />
                Inspired by
                <a href="https://robwalker.substack.com/" target="_blank">The art of noticing</a>
            </div>
        </main>
    </div>
</div>

<style>
    #title {
        position: fixed;
        z-index: 10;
        top: 0;
        left: calc(50% - 200px);
        width: 400px;
        background: rgba(255, 255, 255, 0.6);
        text-align: center;
        border-radius: 5px;
    }

    .info {
        display: none;
        margin-top: 3rem;
        font-size: 1.5rem;
        padding-bottom: 3rem;
        opacity: 0;
        transform: opacity 1s ease-out;
    }

    .is-open {
        display: flex;
        flex-direction: column;
        opacity: 1;
    }

    .contrib {
        margin-top: 1.5rem;
        font-size: 1rem;
    }

    .space {
        width: 1.2em;
    }

    h1 {
        padding: 0rem 0.1rem;
        line-height: 0.5rem;
    }

    button {
        cursor: pointer;
    }

    button > div {
        display: flex;
        justify-items: center;
        align-items: baseline;
    }

    button:hover {
        color: rgb(95, 95, 95);
    }

    .info-icon {
        fill: currentColor;
        opacity: 0.5;
        padding-left: 1rem;
    }

    @media screen and (max-width: 390px) {
        h1 {
            font-size: 2rem;
        }

        #title {
            width: 200px;
            left: calc(50% - 100px);
        }

        .info {
            font-size: 1rem;
        }

        .contrib {
            font-size: 0.8rem;
        }
    }
</style>
