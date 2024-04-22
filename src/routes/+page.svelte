<script lang="ts">
    let currentNumber = $state(0);
    let storedNumber = $state(0);
    let numberChanged = $state(false);

    let input: HTMLInputElement;

    const operations = {
        "+": () => {storedNumber += currentNumber; },
        "-": () => {storedNumber -= currentNumber; },
        "*": () => {storedNumber *= currentNumber; },
        "/": () => {if (currentNumber !== 0) storedNumber /= currentNumber; else alert("Not in this universe!"); },
    } as const;

    let currentOp: Operator | null = $state(null);

    type Operator = keyof typeof operations;

    function finishOp() {
        currentNumber = storedNumber;
        input.focus();
        input.select();
    }

    function calc() {
        if (currentOp === null) return;
        operations[currentOp]();
        finishOp()
        currentOp = null;
    }

    function setOp(op: Operator) {
        input.blur();
        if (numberChanged) {
            calc()
        }
        currentOp = op;
        input.focus();
        input.select();
    }

    function clear() {
        currentNumber = 0;
        storedNumber = 0;
        currentOp = null;
    }
</script>

<h1>Calculate It!</h1>
<input type="number" onchange={() => {numberChanged = true; }} bind:value={currentNumber} bind:this={input} />

<button type="button" class:current={currentOp == "+"} onclick={()=>setOp("+")} >+</button>
<button type="button" class:current={currentOp == "-"} onclick={()=>setOp("-")} >-</button>
<button type="button" class:current={currentOp == "*"} onclick={()=>setOp("*")} >*</button>
<button type="button" class:current={currentOp == "/"} onclick={()=>setOp("/")} >/</button>
<button type="button" onclick={calc} >=</button>
<button type="button" onclick={clear} >C</button>

<style>
    button.current {
        border: 3px magenta solid;
    }
</style>