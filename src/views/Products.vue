<template>
    <div class="searchBox">
        <input type="text" v-model="productName" placeholder="Product name...">
        <div class="searchList" v-if="productName.length">
            <div @click.prevent.left="() => handleProductClick(product)" class="product" v-for="product in productsData" :key="productsData.indexOf(product)">
                <img :src="product.thumbnail">
                <div class="info">
                    <h3>{{product.title}} - ${{product.price}}</h3>
                    <p>{{product.description.slice(0, 70) + "..."}}</p>
                </div>
            </div>
        </div>
    </div>

    <div class="productWrapper" v-if="productClicked">
        <div class="product">
            <h1 class="title">{{productClicked.title}}</h1>
            
            <div class="imageGallery">
                <button @click.prevent.left="previousImage" class="previous  imageBtn"><</button>
                <div class="images">
                    <img :src="productClicked.images[productImageId]">
                </div>
                <button @click.prevent.left="nextImage" class="next imageBtn">></button>
            </div>
            
            <p class="desc">{{productClicked.description}}</p>
            <div class="info">
                <p class="rating">⭐{{productClicked.rating}}</p>
                <p class="price">Price: $ {{productClicked.price}}</p>
            </div>
            <button @click.left.prevent="addToCart" class="addCart">Add to cart</button>
            
        </div>
    </div>

    <div class="notFound" v-else-if="productName != ''">
        <h1> ): Product not found :(</h1>
    </div>

</template>

<script >
    import { ref, watchEffect } from "vue"

    export default {
        emits: ['addCart'],
        props: ['productList'],
        setup(props, { emit }){

            const productName = ref("")
            const productCategory = ref("")
            const productsData = ref([])
            const productClicked = ref()
            const productImageId = ref(0)
            
            const fetchData = async () => {
                try{
                    const response = await fetch ("https://dummyjson.com/products/search?q=" + String(productName.value) + "&select=title,price,description,images,thumbnail,rating")

                    if(!response.ok){
                        throw new Error("Could not fetch")
                    }

                    const data = await response.json()

                    productsData.value = data.products

                }catch(error){
                    console.log(error)
                }
            }

            const handleProductClick = (product) => {
                productName.value = ""
                productClicked.value = product
                productImageId.value = 0
            }

            const previousImage = () => {
                if(productImageId.value > 0){
                    productImageId.value -= 1
                }
            }

            const nextImage = () => {
                if(productImageId.value < productClicked.value.images.length - 1){
                    productImageId.value += 1
                }
            }

            const addToCart = () => {
                emit('addCart', productClicked.value)
            }

            watchEffect(() =>{
                if(productName.value != ""){
                    fetchData()
                }
            })


            return {productName, addToCart, previousImage, nextImage, productCategory, productImageId, productsData, productClicked, handleProductClick}
        }
    }
</script>

<style lang="scss" scopped>
    .searchBox{
        display: flex;
        flex-direction: row;
        justify-content: center;
        background-color: var(--textColor);
        padding: 20px;
        margin-bottom: 15px;
        position: relative;

        input[type="text"]{
            background-color: var(--backgroundColor);
            border: 2px solid var(--detailsColor);
            color: var(--textColor);
            height: 40px;
        }

        .searchList{
            z-index: 2;
            position: absolute;
            top: 80px;
            max-width: fit-content;
            max-height: 180px;
            overflow-y: scroll;
            background-color: var(--textColor);
            border: 1px solid var(--detailsColor);
            
            .product{
                margin: 10px;
                max-height: 110px;
                max-width: 900px;
                display: flex;
                flex-direction: row;
                background-color: var(--backgroundColor);
                color: var(--textColor);

                filter: brightness(100%);
                transform: scale(1);

                transition-property: filter, transform;
                transition-duration: .3s;

                img{
                    width: 160px;
                    margin: 5px;
                    border: 1px solid var(--detailsColor);
                }

                .info{
                    display: flex;
                    flex-direction: column;

                    h3{
                        margin: 0;
                        margin-left: 10px;
                        font-size: 23px;
                    }

                    p{
                        margin: 0;
                        margin-left: 15px;
                    }

                }

                &:hover{
                    filter: brightness(90%);
                    transform: scale(0.98);
                    cursor: pointer;

                    transition-property: filter, transform;
                    transition-duration: .2s;
                }
            }
        }
    }

    .notFound{
        text-align: center;
        h1{
            font-size: 50px;
        }
    }

    .productWrapper{
        display: flex;
        margin: 10px;
        justify-content: center;

        .product{
            max-width: 75%;
            width: 700px;
            border-left: 5px solid var(--detailsColor);
            border-top: 5px solid var(--detailsColor);
            border-right: 5px solid var(--textColor);
            border-bottom: 5px solid var(--textColor);

            display: flex;
            flex-direction: column;
            justify-content: center;


            .title{
                font-size: 50px;
                margin: 0;
                text-align: center;
                margin-bottom: 10px;
                text-transform: capitalize;
            }   

            .imageGallery{
                display: flex;
                flex-direction: row;
                position: relative;
                padding: 0px 10px;
                justify-content: center;

                .imageBtn{
                    position: absolute;
                    height: 400px;
                    background-color: transparent;
                    color: var(--detailsColor);
                    font-size: 27px;
                    border: none;
                    filter: brightness(100%);

                    &:hover{
                        background-color: var(--textColor);
                        opacity: .7;
                        filter: brightness(120%);
                    }
                }

                .previous{
                    left: 10px;
                }

                .next{
                    right: 10px;
                }
                .images{
                    width: 100%;
                    img{
                        width: 100%;
                        height: 400px;
                    }
                }
                
            }

            .desc{
                text-align: justify;
                padding: 20px 15px;
                margin: 0;
                text-transform: capitalize;
            }

            .info{
                display: flex;
                justify-content: space-around;
                margin-bottom: 5px;
                p{
                    margin: 0;
                    
                }
            }

            .addCart{
                width: 80%;
                margin: 10px auto;
                background-color: var(--detailsColor);
                border: 1px solid var(--textColor);
                color: var(--textColor);
                border-radius: 5px;
                padding: 5px;

                transform: scale(1);
                transition-property: transform;
                transition-duration: .3s;

                &:hover{
                    transform: scale(1.1);
                    transition-property: transform;
                    transition-duration: .2s;
                }
            }
        }

        
    }


    @media(max-width: 500px){
        .searchBox{
            .searchList{
                max-width: 100%;
            }
        }
        .productWrapper{
            .product{
                max-width: 100%;
            }
        }
    }

</style>