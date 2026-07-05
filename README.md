<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ne İzlesem? 🎬</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Segoe UI,Arial,sans-serif;
}

body{
    background:#0f172a;
    color:white;
    min-height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    padding:20px;
}

.container{
    max-width:700px;
    width:100%;
    text-align:center;
}

h1{
    margin-bottom:15px;
}

select,button{
    padding:12px;
    border:none;
    border-radius:10px;
    font-size:16px;
    margin:5px;
}

button{
    cursor:pointer;
}

.card{
    margin-top:25px;
    background:#1e293b;
    padding:20px;
    border-radius:15px;
}

img{
    width:250px;
    max-width:100%;
    border-radius:12px;
    margin:15px 0;
}

a{
    color:#60a5fa;
    text-decoration:none;
}

.loading{
    font-size:20px;
}
</style>
</head>
<body>

<div class="container">

<h1>🎬 Ne İzlesem?</h1>

<p>Bir kategori seç ve rastgele film önerisi al.</p>

<select id="genre">
    <option value="">🎲 Karışık</option>
    <option value="28">🎬 Aksiyon</option>
    <option value="35">😂 Komedi</option>
    <option value="27">😱 Korku</option>
    <option value="878">🚀 Bilim Kurgu</option>
    <option value="53">🕵️ Gerilim</option>
    <option value="10749">❤️ Romantik</option>
    <option value="18">🎭 Dram</option>
    <option value="12">🏴‍☠️ Macera</option>
    <option value="16">🎞️ Animasyon</option>
    <option value="10751">👨‍👩‍👧‍👦 Aile</option>
</select>

<button onclick="filmSec()">Film Öner</button>

<div id="result"></div>

</div>

<script>

const API_KEY = "4d22b51840422705ff88613726c0874f";

async function filmSec(){

    const result = document.getElementById("result");
    const genre = document.getElementById("genre").value;

    result.innerHTML =
        '<div class="card loading">🎬 Film aranıyor...</div>';

    try{

        let url;

        if(genre){
            url =
            `https://api.themoviedb.org/3/discover/movie?api_key=${API_KEY}&language=tr-TR&sort_by=popularity.desc&with_genres=${genre}&page=1`;
        }else{
            url =
            `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=tr-TR&page=1`;
        }

        const response = await fetch(url);
        const data = await response.json();

        const film =
            data.results[Math.floor(Math.random()*data.results.length)];

        const poster =
            film.poster_path
            ? `https://image.tmdb.org/t/p/w500${film.poster_path}`
            : "";

        const imdbReq = await fetch(
        `https://api.themoviedb.org/3/movie/${film.id}/external_ids?api_key=${API_KEY}`
        );

        const imdbData = await imdbReq.json();

        const imdbLink = imdbData.imdb_id
            ? `https://www.imdb.com/title/${imdbData.imdb_id}/`
            : "#";

        result.innerHTML = `
        <div class="card">

            <h2>${film.title}</h2>

            ${poster ? `<img src="${poster}" alt="${film.title}">` : ""}

            <p><b>⭐ Puan:</b> ${film.vote_average}</p>

            <br>

            <p>${film.overview || "Film özeti bulunamadı."}</p>

            <br>

            <a href="${imdbLink}" target="_blank">
                🎬 IMDb Sayfasını Aç
            </a>

        </div>
        `;

    }catch(error){

        result.innerHTML = `
        <div class="card">
            ❌ Film bilgileri alınamadı.
        </div>
        `;
    }
}
</script>

</body>
</html>
