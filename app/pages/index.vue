<template>
    <div class="container-image-hero image-hero">
        <header>
            <div class="container-items-header"></div>
        </header>
        <div class="hero-div-content-text">
            <div class="hero-title">Save The date!</div>
            <div class="hero-date">
                <div class="day">20</div>
                <div>de setembro de 2026</div>
            </div>
        </div>
    </div>
</template>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Lexend:wght@100..900&family=Parisienne&display=swap");
    body {
        font-family: "Lexend", sans-serif;
    }

    .container-items-header {
        display: flex;
        gap: 1rem;
        justify-content: center;
        color: #fff;
        padding: 0.5rem 1rem;
    }

    .container-image-hero {
        width: 100%;
        height: 100svh;
    }
    .hero-div-content-text {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-self: center;
        height: 60%;
        width: 100%;
        text-align: center;
    }
    .hero-title {
        font-family: "Parisienne", cursive;
        font-size: 6rem;
        color: #fff;
        margin-left: 400px;
    }
    .hero-date {
        font-size: 2rem;
        color: #fff;
        margin-left: 400px;
    }
    .day {
        font-size: 4rem;
        text-decoration: underline;
        font-weight: 700;
    }
    .image-hero {
        background-image: url("~/assets/1773593559891.jpg");
        background-size: cover;
        background-position: center 50%;
    }
</style>
