<template>
    <div class="content">
        <div>
            <h1>Lorem ipsum dolor sit amet.</h1>
            <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Corporis, mollitia!</p>
            <p>Lorem, ipsum.</p>
        </div>

        <img src="https://placeholder.co/500x600">

    </div>
    
</template>

<style lang="scss" scoped>
    .content{
        display: flex;
        height: calc(100dvh - 99px);
        div{
            margin: auto 0;
            margin-right: auto;
            margin-left: 60px;
            display: flex;
            flex-direction: column;
        }

        h1{
            font-size: 80px;
            margin: 0;
            max-width: 90%;
        }

        p{
            font-size: 20px;
            margin: 0;
            margin-left: 20px;
            max-width: 80%;
        }
        
        p + p{
            font-size: 16px;
            opacity: .8;
            margin-left: 22px;
        }

        img{
            width: 50%;
        }
    }
    
    footer{
        width: 100dvw;
        padding: 10px 40px;
        background-color: var(--textColor);
        display: flex;
        justify-content: space-evenly;
        flex-direction: row;
        position: absolute;
        bottom: 0;
    }
    
    @media(max-width:600px){
        .content{
            img{
                display: none;
            }
        }
    }
</style>