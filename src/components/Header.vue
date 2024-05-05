<template>
    <div class="discountDiv">
        <p>Use code <label>NEW15</label> to save 15% on your first purchase</p>
    </div>
    
    <header>
        
        <img class="logo" src="https://placeholder.co/160x40">
        <nav class="navBar">
            <RouterLink to="/">Home</RouterLink>
            <RouterLink :to="{ name: 'Products' }">Products</RouterLink>
            <RouterLink :to="{ name: 'Contact' }">Contact</RouterLink>
        </nav>
        <button class="btn pill login"> Login </button>
    </header>
</template>

<script>
import { RouterLink} from 'vue-router'
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
        
        $headerMargin: 10px;
        background-color: var(--textColor);
        .logo{
            margin: $headerMargin;
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