<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ASCII Wings with Pacific Timer</title>
    <style>
        html, body {
            background-color: #000000;
            color: #e6e6e6;
            margin: 0;
            width: 100%;
            height: 100%;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        pre {
            font-family: "Courier New", Courier, monospace;
            font-size: 14px;
            line-height: 1.2;
            white-space: pre;
            margin: 0;
        }

        #timer {
            margin-top: 24px;
            font-family: "Times New Roman", Times, serif;
            font-size: 24px;
            color: #ffffff;
            letter-spacing: 1px;
        }
    </style>
</head>
<body>

<pre>
 .                                                            ,
 |`.                                                        ,'|
 \_`-._                                                  _,-'_/
./ \-._`-._                                          _,-'_,-/ \,
\._/._ `._>`-.__                                __,-'<_,' _,\_,/
/_ \_.`-._\_-._ `--__                      __--' _,-_/_,-',_/ _\
 /._`\_./ _`_./`.-._ `-.                ,-' _,-,'\,_'_ \,_/'_,\
  \._`/ _/ _`_./  _/`\_ `.            ,' _/'\_  \,_'_ \_ \'_,/
   /._`/ _/ _`_./` _/ `\_ `\_      _/' _/' \_ '\,_'_ \_ \'_,\
    \._`/ _/ _`_./` __/  >.  `-.,-'  ,<  \__ '\,_'_ \_ \'_,/
      /_`/._/._`_./` __.`.-\_.-`'-._/-,',__ '\,_'_,\_,\'_\
            `\._`_./`\._/_/'    _   `\_\_,/'\,_'_,/'
</pre>

<div id="timer"></div>

<script>
function getPacificNow() {
    return new Date(
        new Date().toLocaleString("en-US", { timeZone: "America/Los_Angeles" })
    );
}

function getNextJune3Pacific() {
    const nowPT = getPacificNow();
    let year = nowPT.getFullYear();

    let target = new Date(
        new Date(`${year}-06-03T00:00:00`)
            .toLocaleString("en-US", { timeZone: "America/Los_Angeles" })
    );

    if (nowPT >= target) {
        target = new Date(
            new Date(`${year + 1}-06-03T00:00:00`)
                .toLocaleString("en-US", { timeZone: "America/Los_Angeles" })
        );
    }

    return target;
}

function updateTimer() {
    const nowPT = getPacificNow();
    const target = getNextJune3Pacific();
    const diff = target - nowPT;

    const totalSeconds = Math.floor(diff / 1000);
    const seconds = totalSeconds % 60;
    const minutes = Math.floor(totalSeconds / 60) % 60;
    const hours = Math.floor(totalSeconds / 3600) % 24;
    const days = Math.floor(totalSeconds / 86400);

    document.getElementById("timer").textContent =
        `${days} DAYS ${hours} HOURS ${minutes} MINUTES ${seconds} SECONDS`;
}

updateTimer();
setInterval(updateTimer, 1000);
</script>

</body>
</html>
