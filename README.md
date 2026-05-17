<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Happy Birthday 🎂</title>

<style>
    body{
        margin:0;
        height:100vh;
        display:flex;
        justify-content:center;
        align-items:center;
        background:linear-gradient(135deg,#ffb6c1,#ffd6e7);
        overflow:hidden;
        font-family:Arial, sans-serif;
    }

    .container{
        text-align:center;
    }

    h1{
        color:white;
        font-size:3rem;
        margin-bottom:40px;
        text-shadow:2px 2px 10px rgba(0,0,0,0.2);
    }

    .cake{
        position:relative;
        width:250px;
        margin:auto;
    }

    .layer{
        height:70px;
        border-radius:15px;
        background:#ff69b4;
        margin-top:5px;
        position:relative;
    }

    .layer:nth-child(1){
        width:250px;
        background:#ff8fab;
    }

    .layer:nth-child(2){
        width:200px;
        margin:auto;
        background:#ff5d8f;
    }

    .candles{
        position:relative;
        height:80px;
    }

    .candle{
        width:15px;
        height:50px;
        background:white;
        position:absolute;
        top:20px;
        border-radius:5px;
    }

    .flame{
        width:15px;
        height:25px;
        background:orange;
        border-radius:50%;
        position:absolute;
        top:-20px;
        left:0;
        animation:flicker 0.2s infinite alternate;
        box-shadow:0 0 20px orange;
    }

    @keyframes flicker{
        from{transform:scale(1);}
        to{transform:scale(1.1);}
    }

    .message{
        margin-top:30px;
        color:white;
        font-size:1.5rem;
        opacity:0;
        transition:0.5s;
    }

    .show{
        opacity:1;
    }
</style>
</head>

<body>

<div class="container">
    <h1>Happy Birthday 🎉</h1>

    <div class="cake">

        <div class="candles">
            <div class="candle" style="left:40px;">
                <div class="flame"></div>
            </div>

            <div class="candle" style="left:115px;">
                <div class="flame"></div>
            </div>

            <div class="candle" style="left:190px;">
                <div class="flame"></div>
            </div>
        </div>

        <div class="layer"></div>
        <div class="layer"></div>

    </div>

    <div class="message" id="message">
        🎂 Candles blown out! Make a wish ✨
    </div>
</div>

<script>
async function setupMic() {

    try{
        const stream = await navigator.mediaDevices.getUserMedia({ audio:true });

        const audioContext = new AudioContext();
        const microphone = audioContext.createMediaStreamSource(stream);

        const analyser = audioContext.createAnalyser();
        microphone.connect(analyser);

        analyser.fftSize = 256;

        const dataArray = new Uint8Array(analyser.frequencyBinCount);

        function detectBlow(){

            analyser.getByteFrequencyData(dataArray);

            let volume = dataArray.reduce((a,b)=>a+b) / dataArray.length;

            // Blow sensitivity
            if(volume > 45){

                document.querySelectorAll(".flame").forEach(flame=>{
                    flame.style.display = "none";
                });

                document.getElementById("message").classList.add("show");
            }

            requestAnimationFrame(detectBlow);
        }

        detectBlow();

    } catch(err){
        alert("Microphone access is needed to blow out the candles 🎤");
    }
}

setupMic();
</script>

</body>
</html>
