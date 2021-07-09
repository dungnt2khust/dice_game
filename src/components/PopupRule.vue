<template>
    <div
        v-bind:class="getOpenPopup"
        class="wrapper-popup">
        <div class="modal">
            <h3 class="modal__title">Game rule</h3> 
            <p class="modal__content">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Veritatis dicta quaerat fugit rem obcaecati, ea inventore dolor labore similique est ducimus. Magni harum quaerat eum, voluptates placeat laudantium voluptas totam.</p>
            <button 
                v-on:click="$emit('handleConfirm')"
                class="modal__confirm"> Đã hiểu</button>
        </div>
    </div>
</template>

<script>
    export default {
       name: 'popup-rule',
       props: {
           isOpenPopup: {type: Boolean, default: false}
       },
       data() {
           return {};
       },
       methods: {
           
       },
       computed: {
           getOpenPopup() {
               return {
                   'open-popup' : this.isOpenPopup
               }
           }
       }
    }
</script>

<style>
    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
        visibility: hidden;
        opacity: 0;
        transition: all 0.2s ease;
    }
    .wrapper-popup.open-popup {
        visibility: visible;
        opacity: 1;
    }
    .wrapper-popup .modal {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #fff;
        width: 450px;
        padding: 20px;
        border-radius: 4px;
        transition: all 0.3s ease;
    }
    .wrapper-popup.wrapper-popup.open-popup .modal {
        transform: translate(-50%, -50%) scale(1.2);
    }
    .modal__title { 
        font-size: 2rem;
        margin-bottom: 10px;
    }
    .modal__content { 
        font-size: 1.6rem;
        margin-bottom: 20px;
    }
    .modal__confirm {
        font-size: 1.6rem;
        background-color: #333;
        color: #fff;
        border: 2px solid #000;
        padding: 8px 12px;
        transition: all 0.3s ease;
    }
    .modal__confirm:hover {
        background-color: #fff;
        color: #000;
        cursor: pointer;
    }

</style>