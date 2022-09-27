<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Game's Rule</h3> 
            <p>This Decimal Game is designed to give your students practice with adding and subtracting decimals.</p> 
            <button 
                v-on:click="confirm"
                class="confirm">Understand</button> 
        </div>
        <!-- v-on:click="confirm": gọi đến sự kiện khi người chơi click nút understand thì sẽ bắt đầu chơi   -->
    </div>
</template>
<script>
export default {
    name: 'popup-rule', 
    props: {
        isOpenPopup: { type: Boolean, default: false }  // ở đây ta để cho giá trị nguyên thủy là false, khi người dùng click vào NEW GAME  nó sẽ gọi đến lệnh handleNewGame => chuyển giá trị thành True và hiện popup lên 
    },
    data() {
        return {
        }
    },
    methods: {
        confirm() {// methods này truyền event ra parent (App.vue)
            console.log('confirm trong PopupRule.vue');
            // khi click vào nút Understand nó sẽ kích hoạt lên sự kiện handleConfirm của App.vue 
            this.$emit('handleConfirm');   //dùng để phát 1 sự kiện ra bên ngoài
        }
    },
    computed: {
        getClassPopup: function() {
            return {
                'open-popup': this.isOpenPopup  // thuộc tính chính là tên class = open-popup, điều kiện xảy ra chính là this.isOpen, nếu this.isOpenpopup = true => sẽ có thêm class là open-popup, ngược lại sẽ không có 
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
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }
    /* trạng thái đầu ta sẽ để cho wrapprer-popup ẩn đi ( opacity: 0; visibility: hidden;) */
    /* khi có thêm class open-popup thì mới bắt đầu hiện lên */
    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
    }
    .rule {
        background-color: #fff;
        padding: 20px;
        position: absolute;
        width: 350px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        font-family: Arial, Helvetica, sans-serif;
        transition: all .3s ease;
    }
    /* Scale() sử dụng để thay đổi độ rộng và chiều cao của phần tử. Hiểu một cách đơn giản là bạn có thể zoom phần tử to lên hoặc nhỏ lại tùy ý, với x là zoom chiều ngang và y là zoom chiều dọc 
     -> class open-popup*/
     /*  trạng thái đầu nó có scale là 1.2, trạng thái sau ta sẽ để scale(1) */
    .open-popup .rule {
        transition: translate(-50%, -50%) scale(1);
    }
    /* scale -> class */
    .rule h3 {
        margin-bottom: 10px;
    }
    .rule .confirm {
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border: 2px solid #333;
        background-color: #fff;
        transition: all .3s ease;
    }
    .rule .confirm:hover {
        color: #fff;
        background-color: black;
    }
    /* kết hợp Css3 với Vuejs để tạo ra hiệu ứng Zoom in - Zoom out */
</style>
