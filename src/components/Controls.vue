<template>
    <div class="wrapper-control">
        <!-- Những nút nhấn trong trò chơi cho người dùng tương tác -->
        <button
            v-on:click="newGame"
            class="control btn-new"><i class="ion-ios-plus-outline"></i>New game</button>
        <button 
            v-on:click="rollDice"
            class="control btn-roll"><i class="ion-ios-loop"></i>Roll dice</button>
        <button 
            v-on:click="$emit('handleHoldScore')"
            class="control btn-hold"><i class="ion-ios-download-outline"></i>Hold</button>
        <!-- Hold Score khá đơn giản chỉ là cộng dồn điểm Current vào điểm gốc nên ta có thể viết trực tiếp vào v-on cũng OK  -->
        <!-- 2 cách viết dưới đây sẽ tương đương nhau  -->
        <input 
            v-bind:disabled="isPlaying"
            v-bind:value="finalScore"
            v-on:input="$emit('handleChangeFinalScore', $event)"
            type="number" placeholder="Final score" class="final-score">
        <!-- <input 
            v-bind:value="valueDemo"  // đẩy data vào
            v-on:input="valueDemo = $event.target.value"   //lấy data về 
            type="number" placeholder="Final score" class="final-score">
        </div> -->
        <!-- $event.target.value cách viết tắt lợi dụng biến Event mà không cần tạo thêm thuộc tính handleInput (và viết ở dưới method) -->
        <!--1, ràng buộc dữ liệu (thường dùng trong các thuộc tính HTML) 
                =>sử dụng chỉ thị v-bind : nếu có bất cứ thay đổi nào diễn ra sau đó nó không gán được giá trị mới vào biến này, chỉ có ràng buộc 1 chiều từ data vào trong input  )
                Nếu sử dụng v-bind: ràng buộc dữ liệu 1 chiều thì ta nên có thêm function để xử lý dữ liệu được nhập vào 
            2, ràng buộc dữ liệu 2 chiều 
            => sử dụng chỉ thị v-model: khi ta sửa lại sữ liệu thì nó sẽ tự động gán lại giá trị cho biến, chiều từ data => input và ngược lại từ input vào data   
            - nếu như nó không là data trực tiếp từ component Controls => thì nó không được phép thay đổi 
        -->
    </div>
</template>
<script>
export default {
    name: 'controls',
    props: {
        //finalScore: { type: Number, default: 100 }
        // doi voi kieu du lieu cua 1 Props truyen vao，ta co quyen khai bao thanh nhieu kieu du lieu khac nhau thay vi 1 gia tri mac dinh 
        finalScore: { type: [Number, String], default: 20 },
        isPlaying: { type: Boolean, default: false}
    }, 
    data() {
        return {
            valueDemo: 20
        }
    }, 
    methods: {
        // ta có thể viết như  dưới (tạo ra thêm 1 hàm) hoặc có 1 cách viết tắt khác bằng cách lợi dụng biến event 
        // handleInput(e) {
        //     this.valueDemo = e.target.value;
        //     //console.log(e.target.value);   // lấy ra được giá trị final core - điểm cuối cùng  mà người chơi (tự nhập vào) phải đạt được để dành chiến thắng 
        // }, 
        newGame() {
            // khi hàm newGame được gọi, control sẽ chạy 
            console.log('newGame từ Controls.vue');
            // sau đó nó sẽ kích hoạt lên sự kiện handleNewGame của App.vue
            this.$emit('handleNewGame');        
        },
        rollDice() {
            console.log('rollDice từ Controls.vue');
            this.$emit('handleRollDice');
        }
    }
}
</script>
<style>
/**********************************************
*** Control
**********************************************/
.control {
    position: absolute;
    width: 200px;
    left: 50%;
    transform: translateX(-50%);
    color: #555;
    background: none;
    border: none;
    font-family: Lato;
    font-size: 20px;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 300;
    transition: background-color 0.3s, color 0.3s;
}
.control.disable {
    pointer-events: none;
}
.control:hover { font-weight: 600; }
.control:hover i { margin-right: 20px; }
.control:focus {
    outline: none;
}
.control i {
    color: #42b983;
    display: inline-block;
    margin-right: 15px;
    font-size: 32px;
    line-height: 1;
    vertical-align: text-top;
    margin-top: -4px;
    transition: margin 0.3s;
}
.btn-new { top: 45px;}
.btn-roll { top: 403px;}
.btn-hold { top: 467px;}
.final-score {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    top: 520px;
    color: #555;
    font-size: 18px;
    font-family: 'Lato';
    text-align: center;
    padding: 10px;
    width: 160px;
    text-transform: uppercase;
}
.final-score:focus { outline: none; }
</style>
