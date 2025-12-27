
<html lang="en">
<head>
<meta charset="UTF-8">
<title>HEAT DEATH OF THE UNIVERSE</title>

<style>
    * {
        box-sizing: border-box;
        outline: none;
    }

    html, body {
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
        background-color: #000000;
        font-family: "Times New Roman", Times, serif;
        color: #7a0000;
    }

    body {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .container {
        text-align: center;
    }

    h1 {
        font-size: 2.4rem;
        letter-spacing: 0.45em;
        margin-bottom: 3rem;
        color: #6b0000;
        text-shadow: 0 0 8px #2a0000;
    }

    .countdown {
        display: grid;
        grid-template-columns: repeat(5, auto);
        gap: 3rem;
    }

    .unit {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .value {
        font-size: 2.6rem;
        color: #8a0000;
        text-shadow: 0 0 6px #240000;
    }

    .label {
        margin-top: 0.6rem;
        font-size: 0.7rem;
        letter-spacing: 0.25em;
        color: #3f3f3f;
    }
</style>
</head>
<body>

<div class="container">
    <h1>HEAT DEATH OF THE UNIVERSE</h1>

    <div class="countdown">
        <div class="unit">
            <div class="value" id="millennia"></div>
            <div class="label">MILLENNIA</div>
        </div>
        <div class="unit">
            <div class="value" id="years"></div>
            <div class="label">YEARS</div>
        </div>
        <div class="unit">
            <div class="value" id="days"></div>
            <div class="label">DAYS</div>
        </div>
        <div class="unit">
            <div class="value" id="hours"></div>
            <div class="label">HOURS</div>
        </div>
        <div class="unit">
            <div class="value" id="seconds"></div>
            <div class="label">SECONDS</div>
        </div>
    </div>
</div>

<script>
    const STORAGE_KEY = "heat_death_realtime";

    const DEFAULT_STATE = {
        millennia: 417263,
        years: 842,
        days: 193,
        hours: 11,
        seconds: 27,
        lastUpdate: Date.now()
    };

    function loadState() {
        try {
            const saved = localStorage.getItem(STORAGE_KEY);
            return saved ? JSON.parse(saved) : { ...DEFAULT_STATE };
        } catch {
            return { ...DEFAULT_STATE };
        }
    }

    function saveState() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }

    let state = loadState();

    const el = {
        millennia: document.getElementById("millennia"),
        years: document.getElementById("years"),
        days: document.getElementById("days"),
        hours: document.getElementById("hours"),
        seconds: document.getElementById("seconds")
    };

    function decrementOneSecond() {
        state.seconds--;

        if (state.seconds < 0) {
            state.seconds = 59;
            state.hours--;
        }

        if (state.hours < 0) {
            state.hours = 23;
            state.days--;
        }

        if (state.days < 0) {
            state.days = 364;
            state.years--;
        }

        if (state.years < 0) {
            state.years = 999;
            state.millennia--;
        }
    }

    function applyElapsedTime() {
        const now = Date.now();
        let elapsedSeconds = Math.floor((now - state.lastUpdate) / 1000);

        while (elapsedSeconds > 0) {
            decrementOneSecond();
            elapsedSeconds--;
        }

        state.lastUpdate = now;
    }

    function render() {
        el.millennia.textContent = state.millennia;
        el.years.textContent = state.years;
        el.days.textContent = state.days;
        el.hours.textContent = state.hours.toString().padStart(2, "0");
        el.seconds.textContent = state.seconds.toString().padStart(2, "0");
    }

    function tick() {
        decrementOneSecond();
        state.lastUpdate = Date.now();
        render();
        saveState();
    }

    // Apply time passed while page was closed
    applyElapsedTime();
    render();
    saveState();

    setInterval(tick, 1000);
</script>

</body>
</html>
