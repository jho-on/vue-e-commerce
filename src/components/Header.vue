<template>
    <div class="discountDiv">
        <transition name="marquee">        
            <p>Use code <label>NEW15</label> to save 15% on your first purchase</p>
        </transition>
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
            <label>Username: <input required v-model="username" type="text" maxlength="16"></label>
            <label>Password: <input required type="password" v-model="password"></label>
            <p>Password must contain: </p>
            <span>
                <p class="passwordRule" ref="lowercase">A <b>lowercase</b> letter</p>
                <p class="passwordRule" ref="uppercase">A <b>uppercase</b> letter</p>
                <p class="passwordRule" ref="special">A <b>special</b> character</p>
                <p class="passwordRule" ref="number">A <b>numeric</b> digit</p>
                <p class="passwordRule" ref="characters">Minimun <b>8</b> characters</p>
            </span>
            <input class="btn" type="submit" @click.prevent.left="submitLogin">
        </div>
    </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import { RouterLink} from 'vue-router'

const isLogging = ref(false)
const isLogged = ref(false)

const username = ref("")
const password = ref(undefined)

const number = ref("")
const lowercase = ref("")
const uppercase = ref("")
const characters = ref("")
const special = ref("")

const profilePic = ref("")
let test = false;

function handleLoginClick(){
    isLogging.value = true
}

function saveProfilePic(event){
    console.log(event.target.files[0])
}   

function testValid(target, validationRule, validationResponse){
    if(validationResponse == null){
        return
    }
    if(target.match(validationRule)){
        validationResponse.classList.remove("invalid")
        validationResponse.classList.add("valid")
        return true
    }else{
        validationResponse.classList.remove("valid")
        validationResponse.classList.add("invalid")
    }
}

watchEffect(() => {
    test = false
    if(password.value != undefined){
        test = testValid(password.value, /[a-z]/g, lowercase.value)
        
        test = testValid(password.value, /[A-Z]/g, uppercase.value)

        test = testValid(password.value, /[0-9]/g, number.value)

        test = testValid(password.value, /[!@#\$%\&*\)\(\+=._-]/g, special.value)

        if(characters.value != null){
            if(password.value.length >= 8){
                test = true
                characters.value.classList.remove("invalid")
                characters.value.classList.add("valid")
            }else{
                characters.value.classList.remove("valid")
                characters.value.classList.add("invalid")
            }
        }
    }

})

function submitLogin(){
    if(test){
        isLogging.value = false
        isLogged.value = true
    }else{
        return
    }
}


</script>

<style lang="scss" scoped>
    
    .discountDiv{
        text-align: center;
        background-color: var(--detailsColor);
        font-weight: 600;
        padding: 5px;
        p{
            margin: 0;
            label{
                background: var(--textColor);
                color: var(--detailsColor);
                padding: 0px 5px;
            }
        }
    }
    
    header{
        z-index: 3;
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
        background-color: #333333fc;
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
            height: fit-content;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 15px;

            p{
                margin: 5px;
            }

            span{
                p{
                    margin: 2px;
                }
                .passwordRule{
                    &.invalid{
                        color: red;
                    }

                    &.valid{
                        color: green;
                    }
                }
            }

            label{
                font-weight: 900;
                font-size: 22px;
            }

            input[type="text"], input[type="password"]{
                margin: 5px 10px;
                width: calc(100% - 20px);
            }
            input[type="submit"]{
                margin-top: 15px;
                font-weight: 500;
                transform: scale(1);
                filter: brightness(100%);
                transition-property: transform, filter;
                transition-duration: .3s;

                &:hover{
                    transform: scale(1.1);
                    filter: brightness(110%);

                    transition-property: transform, filter;
                    transition-duration: .3s;
                }
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