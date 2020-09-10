<template>
  <div class="top-bar">
    <b-navbar toggleable="lg" class="top-bar-info">
        <b-navbar-brand class="logo-title">Where in the world?</b-navbar-brand>
        <div class="ml-auto dark-light-mode">
            <img class="theme-image" src="../assets/images/moon.png" alt="dark mode image">
            <b-navbar-brand class="dark-light-mode-toggle" @click="toggleTheme">Dark Mode</b-navbar-brand>
        </div>
    </b-navbar>
  </div>
</template>

<script>
export default {
    name: "MainTopBar",
    methods: {
        toggleTheme() {
            console.log("swithing themes")
            let lightTheme = document.querySelector('.light-theme');
            let darkTheme = document.querySelector('.dark-theme');
            let themeImage = document.querySelector('.theme-image');
            let body = document.querySelector('body');

            if(lightTheme) {
                themeImage.classList.add('light-image');
                themeImage.classList.remove('dark-image');
                body.classList.add('dark-theme');
                body.classList.remove('light-theme');
            }
            if(darkTheme) {
                themeImage.classList.add('dark-image');
                themeImage.classList.remove('light-image');
                body.classList.add('light-theme');
                body.classList.remove('dark-theme');
            }
        }
    }
}
</script>

<style scoped>

.light-image {
    filter: brightness(0) invert(1);
}
.dark-image {
    filter: brightness(1) invert(0);
}

.dark-light-mode-toggle {
    margin-left: 10px;
    font-weight: 600;
    font-size: 17px;
}

.dark-light-mode-toggle:hover {
    cursor: pointer;
}

.dark-light-mode img {
    max-width: 100%;
    width: 15px;
    margin: auto auto 5px 10px;
}

.logo-title {
    font-size: 26px;
    font-weight: 800;
    margin-left: 15px;
}

.top-bar {
    background-color: rgb(255,255,255);
    box-shadow: 0 2px 2px -2px rgba(0,0,0,.2);
}

@media (min-width: 768px) {
    .top-bar-info {
        width: 1415px;
        margin: auto;
    }
}

@media (max-width: 768px) {
    .top-bar-info {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
    }
    .top-bar-info img {
        margin-right: 5px;
    }
    .top-bar-info .logo-title {
        font-size: 15px;
    }
    .top-bar-info div {
        margin: auto;
    }
    .top-bar-info {
        width: 375px;
        margin: auto;
    }
}
</style>