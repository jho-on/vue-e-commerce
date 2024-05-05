<template>
    <div class="searchBox">
        <input type="text" v-model="productName" placeholder="Product name...">

        <!-- <label>Category: 
            <select v-model="productCategory">
                <option></option>
            </select>
        </label> -->

    </div>
    <div class="products" v-if="productsData.length">
        <div class="product" v-for="product in productsData" :key="product.id">

            <h1 class="title">{{product.title}}</h1>
            <img class="image" :src="product.thumbnail">
            <p class="desc">{{product.description.slice(0, 50) + "..."}}</p>

            <div class="info">
                <p class="rating">⭐{{product.rating}}</p>
                <p class="price">Price: $ {{product.price}}</p>
            </div>
            
        </div>
    </div>

    <div class="notFound" v-else-if="productName != ''">
        <h1> ): Product not found :(</h1>
    </div>

</template>

<script>
    import { ref, watchEffect } from "vue"

    export default {
        name: 'Products',
        setup(){
            const productName = ref("")
            const productCategory = ref("")
            const productsData = ref([])
            
            
            const fetchData = async () => {
                try{
                    const response = await fetch ("https://dummyjson.com/products/search?q=" + String(productName.value))

                    if(!response.ok){
                        throw new Error("Could not fetch")
                    }

                    const data = await response.json()

                    productsData.value = data.products

                }catch(error){
                    console.log(error)
                }
            }

            watchEffect(() =>{
                if(productName.value != ""){
                    fetchData()
                }
            })


            return {productName, productCategory, productsData}
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
        input[type="text"]{
            background-color: var(--backgroundColor);
            border: 2px solid var(--detailsColor);
            color: var(--textColor);
            height: 40px;
            margin-right: 10px;
        }
    }

    .notFound{
        text-align: center;
        h1{
            font-size: 50px;
        }
    }

    .products{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        gap: 1em;
        
        .product{
            border-right: 5px solid var(--textColor);
            border-bottom: 5px solid var(--textColor);
            border-left: 5px solid var(--detailsColor);
            border-top: 5px solid var(--detailsColor);

            height: 600px;
            width: 520px;

            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;

            .title{
                text-align: center;
                text-transform: uppercase;
                font-size: 30px;
                color: var(--textColor);
                padding: 5px 10px;
                margin: 20px  10px;
                border-radius: 10px;
            }

            .image{
                max-width: 300px;
                max-height: 300px;
            }

            .desc{
                opacity: .8;
            }

            .info{
                display: flex;
                flex-direction: row;
                justify-content: space-between;
                width: 100%;
                padding: 0px 40px;

            }
        }
    }
</style>