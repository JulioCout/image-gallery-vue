<script>
export default {
  data(){
    return {
      images: [],
      page: 2,
      inputValue: "",
      alreadySearched: false
    }
  },
  methods: {
    async imageSearch(){
      const response = await fetch(`https://api.unsplash.com/search/photos/?query=${this.inputValue}&client_id=${import.meta.env.VITE_UNSPLASH_API_KEY}`)
      const data = await response.json()
      this.images = data.results
      this.page = 2
      this.alreadySearched = true
    },
    async addImages(){
      const response = await fetch(`https://api.unsplash.com/search/photos?page=${this.page}&query=${this.inputValue}&client_id=${import.meta.env.VITE_UNSPLASH_API_KEY}`)
      const data = await response.json()
      this.images = this.images.concat(data.results)
      this.page++
    },
    async startImages(){
      const response = await fetch(`https://api.unsplash.com/photos?client_id=${import.meta.env.VITE_UNSPLASH_API_KEY}`)
      this.images = await response.json()
    }
  },
  mounted(){
    this.startImages()
  }
}
</script>

<template>
  <div class="App">
      
      <header class="App-header">

        <div class="search">
          <h1>Galeria de Imagem com API do Unsplash</h1>
          
          <div class="search-input">
            <input type="text" placeholder="Buscar Imagem" v-model="inputValue" @keyup.enter="imageSearch" />
            <button type="submit" @click="imageSearch">Buscar</button>
          </div>

        </div>

      </header>

      <div class="images-wrapper">
        <img v-for="(image, index) in this.images" loading="lazy" :key="index" :src="image.urls.regular" :alt="image.alt_description" />
      </div>

      <div class="addButton-section">
        <button v-if="this.alreadySearched"  @click="addImages">Mostrar Mais</button>
      </div>

      <footer>
        <p>Feito por <a href="https://www.juliocoutinho.dev" target="_blank" rel="noreferrer">Julio Coutinho</a></p>
      </footer>

    </div>
</template>

<style scoped lang="scss">
.App {
    display: grid;
    justify-items: center;

    
    .App-header{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        width: 100vw;
        min-width: 37rem;
        height: 50vh;
        min-height: 40rem;
        
        background-image: url("https://source.unsplash.com/random?a=1");
        background-size: cover;

        .search {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 2rem;
            gap: 4rem;

            text-align: center;

            width: 32rem;
            height: 28.8rem;

            backdrop-filter: blur(3px) saturate(100%);
            -webkit-backdrop-filter: blur(3px) saturate(100%);
            background-color: rgba(255, 255, 255, 0.31);
            border-radius: 7px;

            h1 {
                font-size: 3.6rem;
                line-height: 4rem;

            }

            .search-input {
                display: flex;
                gap: 3rem;

                input {
                    width: 80%;
                    padding: 8px;
                    font-size: 14px;
                    border-color: #CCCCCC;
                    background-color: #FFFFFF;
                    color: #242424;
                    border-style: solid;
                    border-radius: 5px;
                    box-shadow: 0px 0px 1px rgba(66,66,66,.64);
                }

                button {
                    cursor: pointer;
                    padding: 12.5px 30px;
                    border: 0;
                    border-radius: 5px;
                    background-color: #2ba8fb;
                    color: #ffffff;
                    font-weight: Bold;
                    transition: all 0.5s;
                    -webkit-transition: all 0.5s;
                  }
                  
                  button:hover {
                    background-color: #6fc5ff;
                    box-shadow: 0 0 20px #6fc5ff50;
                  }
                  
                  button:active {
                    background-color: #3d94cf;
                    transition: all 0.25s;
                    -webkit-transition: all 0.25s;
                    box-shadow: none;
                  }
            }
        }

    }

    .images-wrapper{
        padding: 3.2rem;
        display: grid;
        align-items: center;
        gap: 2rem;

        img{
            border-radius: 2rem;
            width: 100%;
            object-fit: cover;
        }
    }

    .addButton-section {
        padding: 4rem;

        .hide {
            display: none;
        }

        button {
            appearance: button;
            background-color: #1899D6;
            border: solid transparent;
            border-radius: 16px;
            border-width: 0 0 4px;
            box-sizing: border-box;
            color: #FFFFFF;
            cursor: pointer;
            display: inline-block;
            font-size: 15px;
            font-weight: 700;
            letter-spacing: .8px;
            line-height: 20px;
            margin: 0;
            outline: none;
            overflow: visible;
            padding: 13px 19px;
            text-align: center;
            text-transform: uppercase;
            touch-action: manipulation;
            transform: translateZ(0);
            transition: filter .2s;
            user-select: none;
            -webkit-user-select: none;
            vertical-align: middle;
            white-space: nowrap;
           }
           
           button:after {
            background-clip: padding-box;
            background-color: #1CB0F6;
            border: solid transparent;
            border-radius: 16px;
            border-width: 0 0 4px;
            bottom: -4px;
            content: "";
            left: 0;
            position: absolute;
            right: 0;
            top: 0;
            z-index: -1;
           }
           
           button:main, button:focus {
            user-select: auto;
           }
           
           button:hover:not(:disabled) {
            filter: brightness(1.1);
           }
           
           button:disabled {
            cursor: auto;
           }
           
           button:active:after {
            border-width: 0 0 0px;
           }
           
           button:active {
            padding-bottom: 10px;
           }
    }

    footer{
        p{
            padding: 2rem;
            font-size: 1.4rem;
        }
    }
}

@media screen and (min-width:770px) {
    .App {
        .App-header {
            .search {
                width: 48rem;
            }
        }

        .images-wrapper{
            grid-template-columns: 1fr 1fr 1fr;
            
            img {
                height: 33rem;
            }
        }
    }
}
</style>
