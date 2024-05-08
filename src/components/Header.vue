<template>
    <div class="discountDiv">
        <p>Use code <label>NEW15</label> to save 15% on your first purchase</p>
    </div>
    
    <header>
        
        <img class="logo" src="https://placeholder.co/160x40">
        <nav class="navBar">
            <RouterLink to="/">Home</RouterLink>
            <RouterLink :to="{ name: 'Products' }">Products</RouterLink>
            <RouterLink :to="{ name: 'Cart' }" v-if="isLogged">Cart</RouterLink>
        </nav>
        <button v-if="!isLogged" class="btn pill login" @click.left="handleLoginClick"> Login </button>
        <div v-else class="userProfile">
            <img  src="https://placeholder.co/80">
            <p>{{ username }}</p>
        </div>
    </header>

    <div class="loginScreen" v-if="isLogging">
        <div>
            <label>Username: <input v-model="username" type="text" maxlength="10"></label>
            <label>Password: <input type="password"></label>
            <input type="submit" @click.prevent.left="submitLogin">
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import { RouterLink} from 'vue-router'

const isLogging = ref(false)
const isLogged = ref(false)
const username = ref("")
const profilePic = ref("")

function handleLoginClick(){
    isLogging.value = true
}

function saveProfilePic(event){
    console.log(event.target.files[0])
}   

function submitLogin(){
    isLogging.value = false
    isLogged.value = true
}
</script>

<style lang="scss" scoped>
    .discountDiv{
        text-align: center;
        background-color: var(--detailsColor);
        font-weight: 600;
        p{
            margin: 0;
            label{
                background: var(--textColor);
                color: var(--detailsColor);
                padding: 0px 5px;
            }
        }
        padding: 5px;
    }
    
    header{
        display: flex;
        flex-direction: row;
        align-items: center;
        
        $headerMargin: 10px;
        background-color: var(--textColor);
        .logo{
            margin: $headerMargin;
            height: 100%;
        }

        .navBar{
            width: 80%;
            margin: $headerMargin;
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: flex-start;
            text-align: center;

            a{
                display: inline-block;
                text-decoration: none;
                color: var(--detailsColor);
                min-width: 80px;
                padding: $headerMargin;
                text-transform: uppercase;
                font-weight: bold;
            }

            a + a{
                border-left: 1px solid;
            }

        }

        .login{
            margin: $headerMargin;
            width: 160px;
            border: none;
        }

        .userProfile{
            color: var(--detailsColor);
            text-transform: uppercase;
            padding: 5px;
            text-align: center;
            p{
                font-size: 20px;
                font-weight: 900;
                margin: 0;
            }
            img{
                border-radius: 50%;
                width: 80px;
                height: 80px;
            }
        }
    }

    .loginScreen{
        background-color: var(--textColor);
        width: 100dvw;
        height: 100dvh;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 2;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        div{
            color: var(--textColor);
            background-color: var(--backgroundColor);
            width: 50%;
            height: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;

            input[type="text"], input[type="password"]{
                margin: 5px 10px;
                width: calc(100% - 20px);
            }
        }
    }

    @media(max-width: 600px){
        header{
            flex-direction: column;
            align-items: center;
            .logo{
                margin: none;
            }
            .navBar{
                width: 100%;
                justify-content: center;

                a + a{
                    border: none;
                }
            }
        }

        
    }
</style>