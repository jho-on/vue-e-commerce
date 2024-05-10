<template> 
    <div class="wrapper">
        <div class="productList" v-if="productsOnCart.length">
            <div class="product" v-for="product in productsOnCart" :key="product">
                <img :src="product.thumbnail">
                <div>
                    <h2>{{product.title}}</h2>
                    <p>$ {{ product.price }}</p>
                </div>
                <button class="cartRemove btn" @click="() => removeFromCart(product)"> x</button>
            </div>
        </div>

        <div class="purchaseSummary">
            <h1>Payment</h1>       
            <h2>Total: $ {{ purchasePrice }}</h2>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue'
export default {
    props: ['productList'],
    emits: ['addCart'],
    setup(props, emits){
        const productsOnCart = ref(props.productList)
        const purchasePrice = ref(0)


        const getFullPrice = () =>{
            purchasePrice.value = 0
            productsOnCart.value.forEach(product => {
            purchasePrice.value += product.price
            });
        }

        getFullPrice()


        const removeFromCart = (target) => {
            props.productList.splice(props.productList.indexOf(target), 1)
            console.log(props.productList)
            getFullPrice()
        }


        return { productsOnCart, purchasePrice, removeFromCart }
    }
}
</script>

<style lang="scss" scopped>
    .wrapper{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .productList{
        width: calc(70% - 20px);
        max-width: 900px;
        background-color: var(--textColor);
        margin: 10px;
        padding: 5px 0px;

        .product{
            display: flex;
            flex-direction: row;
            background-color: var(--backgroundColor);
            margin: 5px 10px;
            border: 1px solid var(--detailsColor);
            position: relative;
            .cartRemove{
                position: absolute;
                right: 0;
                top: 0;
                width: 40px;
            }

            div{
                width: 100%;
                
                display: flex;
                flex-direction: column;
                justify-content: space-between;

                h2{
                    text-transform: uppercase;
                    font-size: 25px;
                    text-align: left;
                    padding: 10px 10px;
                    margin: 0;
                }
                p{
                    padding: 10px 15px;
                    text-align: right;
                    margin: 0;
                }
            }
        
            img{
                aspect-ratio: 1 / 1;
                width: 150px;
                height: 150px;
                padding: 5px;
            }   

            &+.product{
                margin-top: 10px;
            }
        }
    }

    .purchaseSummary{
        width: calc(40% - 20px);
        max-width: 400px;
        height: fit-content;

        border-bottom: 5px solid var(--textColor);
        border-right: 5px solid var(--textColor);
        border-top: 5px solid var(--detailsColor);
        border-left: 5px solid var(--detailsColor);
        
        margin: 10px;
        text-align: center;
        
        h1{
            margin: 10px auto;
            width: 90%;
            text-transform: uppercase;
            border-bottom: 1px solid var(--textColor);
        }

    }

    @media(max-width: 500px){
        .wrapper{
            flex-direction: column;
        }
        .productList{
            width: calc(100% - 20px);
        }
        .purchaseSummary{
            width: calc(100% - 20px);
        }
    }
</style>