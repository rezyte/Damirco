<template>

        <div id='productImg' class='productSection'>
                    <div id="productImgWrapper">
                        <label for="productImgInput">
                            عکس محصول را وارد کنید
                        </label>
                        <div id="images">
                            <div class="increaseImgNum">
                                <label for="">اضافه کردن عکس تا حداکثر ده تا</label>
                                <button @click.prevent='imgNums+=1' class="submit">اضافه کردن عکس</button>
                            </div>


                            <div class="allImagesWrapper">
                                <div class='imagesInput' v-for="item in imgNums" :key='item'>
                                <label :for="item" class='uploadImage'>
                                     <i class="fas fa-plus"></i>
                                    انتخاب عکس
                                   
                                    <input @change='imageChanged($event)' type="file" class='prodctImg' :id='item'>
                                    
                                </label>

                                <div class="imageUrl">
                                    <img class='userProductImg' name=productImages[] src="" alt="">
                                    <transition name='fade' mode='out-in'>
                                        <div @mouseleave='blur($event)' @mouseover='focus($event)' class="imgDetails">
                                            <div class="imgName">
                                                <p>teksmkmkdmskdmskmdksmst</p>
                                            </div>
                                            <div class="imgSize">
                                                <p>test</p>
                                            </div>
                                        </div>
                                    </transition>
                                </div>
                                </div>
                            </div>


                        </div>
                    </div>
                </div>
</template>
<script>
export default {
    data(){
        return{
            imgNums:1
        }
    },
    watch:{
        imgNums:function(value){
            if(value>10)
            {
                this.imgNums=10
            }
        }
    },
    methods:{
        clickImage(){
                const imageInput=document.querySelector("#productImgInput")
                imageInput.click()
            },
            imageChanged(e){
                const clickedInput=e.target
                var reader = new FileReader();
                let output=clickedInput.parentNode.nextElementSibling.childNodes[0]
                
                reader.onload = function()
                {
                    output.src = reader.result;
                }
                reader.readAsDataURL(e.target.files[0]);
                output.style.display='block'

                this.showmgDetailes(output,e.target.files[0])
            },
            async showmgDetailes(img,fileDetailes)
            {
                const imgDetail=img.nextElementSibling
                const imgName=imgDetail.querySelector(".imgName p"),
                      imgSize=imgDetail.querySelector(".imgSize p");
                const fileName=fileDetailes.name.length>15 ? '...'+fileDetailes.name.substring(0,11)  : fileDetailes.name
                imgName.innerText=fileName
                imgSize.innerText=(fileDetailes.size)/1000
            }
            ,
            focus(e){
                const pre=e.target.closest(".imageUrl").childNodes[0]
                pre.classList.add('blury')
            },
            blur(e){
                const pre=e.target.closest(".imageUrl").childNodes[0]
                pre.classList.remove('blury')
            }
    }
}
</script>

<style>
    #productImgWrapper{
        display:flex;
        flex-direction:column
    }
    #productImgWrapper > label{
        width:100%;
        display:flex;
        justify-content: center;
    }
    .imgDetails:hover{
        display:block
    }
    .imgDetails p{
        font-size:14pt;
        padding:5px;
        width:100px;
        overflow: hidden;
        display:flex;
        justify-content: center;
    }
    .imgDetails{
        position: absolute;
        left:50%;
        top:50px;
        display: none;
        flex-direction:column;
        justify-content: center;
        transform: translateX(-50%);
    }
    .imgName{
        background-color: hsla(0,0%,100%,.4);;
        width: 100px;
        height:20px;
        overflow: hidden;
    }
    .imgName p{
        max-width:100px;
        overflow: hidden;
        font-size:11pt;
        text-align: left;
        white-space: nowrap;;
    }
    .uploadImage{
        display:flex;
        align-items: center;
    }
    .imageUrl img{
        display:none;
        width:120px;
        height:120px;
        border-radius: 20px;
    }
    .imageUrl img:hover{
        -webkit-filter: blur(8px);
        -moz-filter: blur(8px);
        -o-filter: blur(8px);
        -ms-filter: blur(8px);
        filter: blur(8px);
    }
    .blury{
        -webkit-filter: blur(8px);
        -moz-filter: blur(8px);
        -o-filter: blur(8px);
        -ms-filter: blur(8px);
        filter: blur(8px);
    }
    .imagesInput{
        width:200px;
        display: flex;
        margin:10px;
        flex-direction:column;
        align-items: center;
    }
    .imagesInput input{
        display:none
    }
    #realImg{
        width:300px;
        height: 300px;
        display:none
    }
    .imagesInput .submit{
        width: max-content;
    }
    #images{
        display: flex;
        width:100%;
        margin-top:10px;
        align-items: center;
        flex-direction:column;
        flex-wrap: wrap;
    }
    .imageUrl{
        width:200px;
        height: 200px;
        margin-top:10px;
        margin-bottom:10px;
        background: cornsilk;
        outline:2px dashed #0061af;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
    }
    .allImagesWrapper{
        display:flex;
        flex-wrap: wrap;
        justify-content: space-between;
    }
    .userProductImg:hover + div{
        display:block
    }
    #productImgInput
    {
        display:none
    }
    .increaseImgNum{
        display:flex;
        flex-direction: column;
        align-items: center;
        margin:10px 0;
    }
    .increaseImgNum .submit{
        margin-top:10px;
    }
    @media (max-width:850px)
    {
        #images{
            flex-direction: column;
            align-items: center;
        }
    }
    .fade-enter-active{
        animation:fadeIn .5s linear
    }
    @keyframes fadeIn {
        from{
            opacity:0;
        }
        to{
            opacity:1
        }
    }
</style>