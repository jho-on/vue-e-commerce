<template>
    <div class="content">
        <div>
            <h1>Lorem ipsum dolor sit amet.</h1>
            <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit.
                 Corporis, mollitia!</p>
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
            font-size: clamp(3vw, 80px, 15vw);
            margin: 0;
            max-width: 90%;
            animation: slideFromLeft .7s ease-out;
        }

        p{
            font-size: 20px;
            margin: 0;
            margin-left: 20px;
            max-width: 80%;
            animation: slideFromLeft .8s ease-out;
        }
        
        p + p{
            font-size: 16px;
            opacity: .8;
            margin-left: 22px;
            animation: slideFromLeft .85s ease-out;
        }

        img{
            width: 50%;
            max-width: 500px;
        }
    }
    
    @keyframes slideFromLeft {
        from{
            transform: translateX(-200px);
        }
        to{
            transform: translateX(0px);
        }
    }




    @media(max-width:600px){
        .content{
            img{
                display: none;
            }
        }
    }
</style>