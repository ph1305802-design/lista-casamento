<template>
    <!-- Hero -->
    <div class="container-body">
        <div class="container-hero flex-col lg:flex-row">
            <div class="flex items-center flex-col gap-4">
                <div class="container-nomes">
                    <h1 class="nomes-noivos">Pedro Henrique</h1>
                    <h2 class="preposicao">&</h2>
                    <h1 class="nomes-noivos">Polyanna</h1>
                </div>
                <div class="container-verse">
                    <p>Colossenses 3:14</p>
                </div>
                <div>
                    <img class="flower-hero" src="~/assets/width_408.webp" alt="" />
                </div>
            </div>
            <div>
                <div class="div-img-hero">
                    <img class="img-hero" src="~/assets/1778711729577.webp" alt="" />
                </div>
            </div>
        </div>
    </div>
    <!-- Crie a seção Save The Date -->
    <section class="section-save-the-date">
        <div>
            <div class="flex flex-col lg:flex-row gap-8 items-center justify-center">
                <div>
                    <p class="text-save-the-date">
                        O grande dia está chegando! Ainda falta um pouco, mas já queremos você com a gente. Já anota aí:
                        essa data é nossa — e queremos você com a gente!
                    </p>
                    <div class="text-center flex flex-col mt-4">
                        <p class="faltam-exatos-save-the-date">Faltam exatos</p>
                        <Timer />
                    </div>
                </div>
                <div>
                    <h2 class="titulo-save-the-date">Save The Date</h2>
                    <p class="subtitulo-save-the-date">20 de setembro de 2026</p>
                    <div class="imgs-save-the-date">
                        <img class="main-img-save-the-date object-cover" src="~/assets/save_the_date_1.webp" alt="" />
                        <img class="flower-save-the-date" src="~/assets/width_478.webp" alt="" />
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Crie a seção dos presentes -->
    <div class="flex flex-col items-center">
        <ListPresents />
    </div>
</template>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Great+Vibes&family=IM+Fell+English:ital@0;1&family=Yeseva+One&display=swap");

    :root {
        --main-bg-color: #f0ca99;
        --main-font-color: #522b1d;
        --main-primary-color: #de5026;
    }
    body {
        font-family: "Lexend", sans-serif;
    }
    .text-save-the-date {
        font-family: "Lexend", sans-serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 18px;
        font-weight: 400;
        max-width: 450px;
    }
    .section-save-the-date {
        padding: 0px 24px;
        margin: 42px auto;
    }
    .titulo-save-the-date {
        font-family: "Yeseva One", serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 52px;
        text-transform: uppercase;
    }
    .subtitulo-save-the-date {
        font-family: "Yeseva One", serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 18px;
    }
    .faltam-exatos-save-the-date {
        font-family: "Yeseva One", serif;
        text-align: center;
        color: var(--main-font-color);
        font-size: 14px;
    }
    .imgs-save-the-date {
        margin-top: 24px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
    .imgs-save-the-date .main-img-save-the-date {
        height: 400px;
    }
    .imgs-save-the-date .flower-save-the-date {
        height: 150px;
        position: relative;
        top: -70px;
        left: -40px;
        margin-right: auto;
        transform: rotate(45deg);
    }
    .container-body {
        background-color: var(--main-bg-color);
        height: 100svh;
        width: 100%;
    }
    .container-nomes {
        text-align: center;
        color: var(--main-font-color);
        font-size: 4rem;
    }
    .nomes-noivos {
        font-family: "Yeseva One", serif;
    }
    .preposicao {
        font-family: "Great Vibes", cursive;
    }
    .flower-hero {
        position: absolute;
        bottom: 0px;
        left: 40px;
        height: min(calc(100vw - 2rem), 400px);
        transform: rotate(-15deg);
    }
    .container-verse {
        width: 100%;
        max-width: 350px;
        font-family: "Great Vibes", cursive;
        text-align: center;
        background-color: var(--main-primary-color);
        font-size: 3rem;
        color: white;
    }
    .container-hero {
        height: 100svh;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 42px;
    }
    .div-img-hero {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 24px;
        background-color: white;
    }
    .img-hero {
        width: 25svw;
        height: 25svw;
        object-fit: cover;
        object-position: 85% 10%;
    }
    @media (max-width: 768px) {
        .titulo-save-the-date {
            font-size: 2.5rem;
        }
        .container-body {
            padding: 24px;
        }
        .img-hero {
            width: 50svw;
            height: 50svw;
        }
        .container-nomes {
            font-size: 3rem;
        }
        .container-verse {
            max-width: 250px;
            font-size: 2rem;
        }
        .flower-hero {
            left: 0px;
            height: min(calc(100vw - 2rem), 200px);
        }
    }
</style>
