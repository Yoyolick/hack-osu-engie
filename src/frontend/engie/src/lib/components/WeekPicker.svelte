<script>
	import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    let date = new Date('Sun Sep 25 2022 00:00:00 GMT-0400 (Eastern Daylight Time)');
    const max = date;
    const min = new Date('Sun Jan 08 2017 00:00:00 GMT-0400 (Eastern Daylight Time)');
    const ending = () => display(offset(6));
    let endTime = ending();

    const day = 86400000; // 24 * 60 ** 2 * 1000

    function display(toFormat) {
        console.log(toFormat);
        return `${toFormat.getMonth() + 1}/${toFormat.getDate()}`;
    }

    function offset(diff) {
        return new Date(date.getTime() + day * diff);
    }
    
    function minus() {
        date = offset(-7);
        endTime = ending();
        dispatch('selected', date);
    }

    function plus() {
        date = offset(7);
        endTime = ending();
        dispatch('selected', date);
    }

    dispatch('selected', date);
</script>

<div class="container">
    <h2>Week of {display(date)}-{endTime}</h2>
    <div style="display:flex:flex-wrap:wrap;align-items:center;">
        <button on:click={minus} disabled={date <= min}>&lt;</button>
        <button on:click={plus} disabled={date >= max}>&gt;</button>
    </div>
</div>

<style>
    .container {
        height: 100%;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        justify-content: center;
    }

    h2 {
        font-size: 3em;
        color: white;
        display: inline-block;
        min-width: 350px;
    }

    button {
        border: none;
        background-color: #333;
        font-size: 2rem;
        padding: 10px 20px;
        color: white;
        position: relative;
        bottom: 3px;
        border-radius: 10%;
        cursor: pointer;
        transition: background .3s;
        margin: 5px;
    }

    button:not([disabled]):hover {
        background-color: #000;
        animation: CircleArrow .3s forwards;
    }

    @keyframes CircleArrow {
        100% {
            border-radius: 20%;
        }
    }

    button:disabled {
        cursor: not-allowed;
        color: #777;
    }
</style>