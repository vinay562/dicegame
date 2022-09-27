<template>
<!-- trong nhung ung dung Single Page Application thi 1 component phai co 1 phan tu bao ngoai, 
neu ta de 2 the <div class="player-panel"> o duoi ma khong co the div bao ngoai, no se hieu la 2 component => bao loi
    the div o duoi day dong vai tro bao lai 
    div chinh la root cua component App -->
 <!-- Note: nếu có nhiều Product thì nên chia nhỏ player ra đẻ viết và cho chạy vòng lặp, 
 trường hợp này chỉ có 2 người chơi nên ta đẻ nguyên như thế này vẫn Ok -->
    <div class="wrapper-player"> 
        <!-- người dùng nào đang chơi thì ở đó sẽ được thêm thuộc tính active 
        ta dùng ràng buộc sữ liệu vào trong class để tự dộng thêm thuộc tính active vào người đang chơi-->
    <!-- v-bind:class="{ active: activePlayer == 0 } là cách viết để thêm class cho thẻ, ở đây ta có điều kiện thêm class active vào chỉ khi activePlayer trùng với　key của nó -->
        <div class="player-panel" v-bind:class="{ 
            active: activePlayer == 0 && !this.isWinner,
            winner: activePlayer ==0 && this.isWinner 
            }">
            <div class="player-name">{{ getNamePlayer(0) }}</div>
            <div class="player-score">{{ scoresPlayer[0] }}</div>
            <div class="player-current-box">
                <div class="player-current-label">Current</div>
                <div class="player-current-score">{{  activePlayer == 0 ? currentScore : 0 }}</div>
            </div>
        </div>
        <div class="player-panel" v-bind:class="{ 
            active: activePlayer == 1 && !this.isWinner,
            winner: activePlayer == 1 && this.isWinner 
            }">
            <div class="player-name">{{ getNamePlayer(1) }}</div>
            <div class="player-score">{{ scoresPlayer[1] }}</div>
            <div class="player-current-box">
                <div class="player-current-label">Current</div>
                <div class="player-current-score">{{ activePlayer == 1 ? currentScore : 0 }}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'players',
    props: {   //khai báo tất cả các dữ liệu từ Parent truyển vào
        //scoresPlayer: Array
        scoresPlayer: { type: Array, default: [0, 0] },  //default: [0, 0]  quy định giá trị mặc định ban đầu 
        currentScore: { type: Number, default: 0 }, //la 1 con số 
        activePlayer: { type: Number, default: 0 },  // la 1 son số 
        isWinner: { type: Boolean, default: false }  //mac dinh gia tri ban dau cua isWinner la false 
    },
    data() {
        return {
        }
    },
    // tao ham de khi co nguoi chien thang thi no se tu dong sua ten nguoi choi do thanh Winner 
    methods: {
        getNamePlayer(index) {
            var defaultName = 'Player ' + (index + 1);  // index + 1 de dam bao sau khi ra ket qua se la Player 1/ Player 2
            console.log(index, defaultName, this.isWinner, this.activePlayer);
            // neu: nguoi hien tai + co nguoi thang cuoc: thay doi ten nguoi choi ve thanh Winner!
            if(this.activePlayer == index && this.isWinner) {
                defaultName = 'Winner!';
            }
            return defaultName;
        } 
    }
}
</script>

<style>
.player-panel {
    width: 50%;
    float: left;
    height: 600px;
    padding: 100px;
    transition: all .3s ease;
    background-color: #fff;
}
.player-name {
    font-size: 40px;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: 100;
    margin-top: 20px;
    margin-bottom: 10px;
    position: relative;
}
.player-score {
    text-align: center;
    font-size: 80px;
    font-weight: 100;
    color: #42b983;
    margin-bottom: 130px;
}
/*  class dành cho người đang chơi */
.active { 
    background-color: #f7f7f7; 
}
.active .player-name { 
    font-weight: 300; 
}
.active .player-name::after {
    content: "\2022";
    font-size: 47px;
    position: absolute;
    color: #42b983;
    top: -7px;
    right: 10px;
}
.player-current-box {
    background-color: #42b983;
    color: #fff;
    width: 40%;
    margin: 0 auto;
    padding: 12px;
    text-align: center;
}
.player-current-label {
    text-transform: uppercase;
    margin-bottom: 10px;
    font-size: 12px;
    color: #222;
}
.player-current-score {
    font-size: 30px;
}
/* style dành cho người chiến thắng */
/* khi 1 người chiến thắng, class active tự động chuyển qua class winner/　đồng thời tên người chơi cũng được sửa lại thành winner */
.winner { 
    background-color: #f7f7f7; 
}
.winner .player-name { 
    font-weight: 300; 
    color: #42b983; 
}
</style>