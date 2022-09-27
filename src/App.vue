<template>
	<div id="app"> 
		<div class="wrapper clearfix">
			<!-- Props: truyền dữ liệu từ Parent => Child-->
			<!-- Enent Up: truyền dữ liệu từ Child => Parent -->
			<players 
			    v-bind:isWinner="isWinner"
				v-bind:activePlayer="activePlayer"
				v-bind:scoresPlayer="scoresPlayer"
				v-bind:currentScore="currentScore"
			/>
			<!-- v-bind: ta ràng buộc scoresPlayer của App.vue vào scoresPlayer của Player (ở đây ta đang đặt tên giống nhau) -->
			<controls  
				v-bind:isPlaying="isPlaying"
				v-bind:finalScore="finalScore"
				v-on:handleHoldScore="handleHoldScore"
				v-on:handleChangeFinalScore="handleChangeFinalScore"
				v-on:handleNewGame="handleNewGame"
				v-on:handleRollDice="handleRollDice"
			/>
			<!-- isPlaying: check xem 2 nguoi co dang choi hay khong, neu dang choi thi khong cho sua FinalScore -->
			<!-- handleChangeFinalScore: chức năng thay đổi lại dữ liệu của FinalScore  -->
			<!-- handleNewgame là tên sự kiện ta tạo thêm, "handleNewGame" là tên của function xử lý sau khi sự kiện đó được kích hoạt-->
			<!-- handleConfirm: nhận và thực thi sự kiện khi người chơi click nút understand thì sẽ bắt đầu chơi -->
		    <dices 
				v-bind:dices="dices"
			/>
			<popup-rule
				v-on:handleConfirm="handleConfirm"
				v-bind:isOpenPopup="isOpenPopup"
			/> 
		</div>
	</div>
</template>
<script>
import Players from './components/Players';
import Controls from './components/Controls';
import Dices from './components/Dices';
import PopupRule from './components/PopupRule'
export default {
    name: 'app',
    data () {
        return {
            isOpenPopup: false, // biến dùng để biết khi nào thì hiển thị Popup 
            isPlaying: false,  //biến dùng để lưu xem user đó có đang chơi game hay không
            activePlayer: 0, // nhận diện xem ai là người chơi hiện tại active = 0 => người chơi là 1 , active = 1 => người chơi là 2 
            scoresPlayer: [0, 0],  //dùng để lưu lại điểm sau cùng
            dices: [2, 6],  // dùng để lưu lại dữ liệu cho con xúc xắc 
            currentScore: 0,   //ta chỉ cần lưu 1 giá trị cho curentScore vì chỉ khi nào ng đó đang chơi mới có điểm tạm thời, ng còn lại có điểm tạm thòi = 0
            finalScore: 100
        }
    },
    components: {
		Players,
		Controls,
		Dices,
        PopupRule
    }, 
	computed: {
		// o day ta khai bao 1 function, va cai function do se tu dong duoc chay bay cu khi nao data ben trong no co su thay doi 
		isWinner() {
			let { scoresPlayer, finalScore } = this;

			if(scoresPlayer[1] >= finalScore || scoresPlayer[1] >= finalScore) {
				//dung cuoc choi
				this.isPlaying = false;  // thay doi gia tri ve false de dung cuoc choi
				return true;   //co nguoi chien thang 
			}
			return false;   //khong co ai chien thang 
		}
	},
    methods: {
        handleChangeFinalScore(e) {
			var number = parseInt(e.target.value);
			if(isNaN(number)) {
				// neu data tra ve la NaN thi can lam 1 thong bao keu nguoi dung nhap so vao 
				this.finalScore = '';
			} else {
				this.finalScore = number;
			}


            //biến event được truyền từ bên control sang 
            //lần này ta vừa phải kích hoạt, vừa phải truyền được data ra ngoài chứ ko đơn giản là chỉ kích hoạt sự kiện như mấy hàm bên dưới 
            //console.log(e.target.value);
			//luc nay, ta ay ra duoc diem so Final cua nguoi choi nhung cho kieu String => phai Add cho no ve kieu Number: ParseInt() : giup dua ve dang so nguyen 
			console.log(parseInt(e.target.value));
			// trong TH nguoi dung khong nhap gi vao, no se tra ve kieu NaN => can Ktra xem co phai la kieu NaN hay khong 
        },
        handleHoldScore() {
            if(this.isPlaying) {
                //console.log('handleHoldScore từ App.vue')
                //muốn truy xuất vào điểm của người chơi T1: scorePlayer[0]=scorePlayer[activePlayer]
                //muốn truy xuất vào điểm của người chơi T2: scorePlayer[1]=scorePlayer[activePlayer]
               // this.scoresPlayer[this.activePlayer] = this.scoresPlayer[this.activePlayer] + this.currentScore; // điểm mới = điểm hiện tại + điểm tạm thời(được lưu bên dưới)
                // cách viết trên sử dụng lại quá nhiều this. ... => thay thế bằng cách viết dưới đây 
                //sử dụng Destructuring là một cú pháp cho phép bạn gán các thuộc tính của một Object hoặc một Array. Điều này có thể làm giảm đáng kể các dòng mã cần thiết để thao tác dữ liệu trong các cấu trúc này.
                let { scoresPlayer, activePlayer, currentScore} = this; 
                let scoreOld = scoresPlayer[activePlayer];
                //C1 hold điểm 
                // let cloneScorePlayer = [...scoresPlayer]; // ở đây ta đang khởi tạo ra 1 mảng mới, mảng này copy dữ liệu lại từ scorePlayer 
                //     cloneScorePlayer[activePlayer] = scoreOld + currentScore;
                // //clone data copy lại dữ liệu 
                // // spread operator (...) dung đé sao chép dữ liệu từ 1 Array/ Object 
                // this.scoresPlayer = cloneScorePlayer; // cách viết này giúp gán lại giá trị cho scorePlayer nhưng không làm thay đổi mảng gốc, sau đó địa chỉ ô nhớ được thay đổi
                // //this.scoresPlayer[activePlayer] = scoreOld + currentScore;
                // //console.log(cloneScorePlayer);
                //C2 hold điểm
                // một cách viết khác để hold điểm bằng cách sử dụng $set(dữ liệu muốn thay đổi,index thay đổi là gì, giá trị mới)
                this.$set(this.scoresPlayer, activePlayer, scoreOld + currentScore );
                // Sau khi người chơi hiện tại Hold điểm của họ thì ta phải đổi lượt chơi , tuy nhien khi hold diem cung can phai kiem tra xem co ai la nguoi thang cuoc hay khong 
				if(!this.isWinner) {  //neu khong co ai thang cuoc => Next
					this.nextPlayer();
				}

            } else {
                alert('Please click on NEW GAME')
            }  
        },
        nextPlayer() {
            //activePlayer = 0 thì sẽ đổi lại là 1 và ngược lại 
            this.activePlayer = this.activePlayer === 0 ? 1 : 0 ;
            this.currentScore = 0; 
        },
        handleConfirm() {
           // console.log('handleConfirm nằm trong App.vue'); 
           this.isPlaying = true;
           this.isOpenPopup = false;  //khi người chơi băt đầu chơi thì popap sẽ được ẩn đi 
           this.activePlayer = 0; // khi người chơi băt đầu chơi, các đàu điểm ban đầu sẽ đều = 0 
           this.dices = [1, 1];
           this.scoresPlayer = [0, 0]; 
           this.currentScore = 0;        
        },
        handleNewGame() {
            console.log('handleNewGame từ App.vue');
            // Hiển thị Popup => Show luật chơi
            this.isOpenPopup=true;
        },
        handleRollDice() {
            console.log('handleRollDice từ App.vue');
            if(this.isPlaying) {
                //nếu isPlaying = true thì ta mới tiến hành xoay Dice 
                // Math.random()  chỉ random ra con số lẻ từ 0-1 nên để hiện random lên 6 mặt của xúc sắc, ta phải nhân với 6 
                // đặt giá trị random là x , 0 <= x <= 1 => 0 <= y (=x*6) <= 6 
                var dice1 = Math.floor(Math.random() * 6) + 1; 
                var dice2 = Math.floor(Math.random() * 6) + 1; 
                this.dices= [dice1, dice2];  // ở đây sau khi xoay random xong ta gán ngược lại giá trị để cho xuc xac xoay theo đúng như yêu cầu
                //console.log(dice1, dice2);
                if(dice1 === 1 || dice2 === 1) {
                    let activePlayer = this.activePlayer; // nếu đưa thẳng this.activePlayer vào setTimeOut => thông báo hiện ra là NaN vì nó không thể trực tiếp chọc ra và lấy this.activePlayer là bao nhiêu /// nếu thay thế bàng arow function thì có thể đưa trục tiếp vào 
                    setTimeout(function() {  //sử dụng setTimeout(bất dồng bộ)  giúp cho khi người chơi quay phải số 1 thì vẫn hiện mặt xúc xắc lên trước sau đó mới hiện thông báo Opps!!
                        alert(`Oops! Player ${activePlayer + 1} has drawn the number 1.` )
                    }, 10)
                    // nếu 1 tronng 2 xúc xắc có gtri 1  tiến hành đổi lượt chơi 
                    this.nextPlayer();
                    // đồng thời reset lại điểm tạm thời(currentcore) về 0 
                } else {
                    // nếu xoay ra những điểm thông thường, ta cộng dồn điểm vào Curent core cho người đang chơi 
                    this.currentScore = this.currentScore + dice1 + dice2;
                }
            } else {   // ngược lại sẽ hiện ra thông báo yêu cầu người chơi click vào nút NEW GAME 
                alert('Please click on NEW GAME')
            }
        }
    }
}
</script>
<style>
/**********************************************
*** GENERAL
**********************************************/
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
body {
 /*lưu ý khi coppy lại code từ HTML/ CSS cần sửa lại đường dẫn đến hình ảnh cần lấy vào. các hình ảnh lúc này nên để hết ở file Public - asets   */
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}
.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
</style>