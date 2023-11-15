<script>
    export default {
        data() {
            return {
                shift_en: "",
                shift_de: "",
                msg_en: "",
                msg_de: "",
                output_en: "",
                output_de: "",
            }
        },
        methods: {            
            encrypt() {
                
                var otherCharacters = '-=~\"\'#$%&*^:<>?/!{(|)}.1234567890\, \\',
                    alphabetLower = "абвгдеёжзийклмнопрстуфхцчшщъыьэюя",
                    alphabetUp = "АБВГДЕЁЖЗИЙКЛМНОПРСТУФЧЦЧШЩЪЫЬЭЮЯ";

                //Take values
                var vm = this,
                    string = vm.msg_en,
                    shiftAmount = vm.shift_en,
                    output_en = vm.output_en = "";
                    

                //Shift alphabet
                var shiftedAlphabetLower = alphabetLower.slice(shiftAmount) + alphabetLower.slice(0, shiftAmount) + otherCharacters,
                    shiftedAlphabetUp = alphabetUp.slice(shiftAmount) + alphabetUp.slice(0, shiftAmount) + otherCharacters;

                alphabetLower += otherCharacters;
                alphabetUp += otherCharacters;
                
                //Comparison simbols
                function simbol(symb, arr) {
                    for (var i = 0; i < arr.length; i++) {
                        if (symb === arr[i]) {
                            return true;
                        }
                    }
                }

                //Encrypt message
                    for (var i = 0; i < string.length; i++) {
                        if (simbol(string[i], alphabetLower)) {
                            var number = alphabetLower.indexOf(string[i]);
                            output_en += shiftedAlphabetLower[number];
                        } else {
                            var number = alphabetUp.indexOf(string[i]);
                            output_en += shiftedAlphabetUp[number];
                        }
                    }
                
                vm.output_en = output_en
            },

            decrypt() {

                var otherCharacters = '-=~\"\'#$%&*^:<>?/!{(|)}.1234567890\, \\',
                    alphabetLower = "абвгдеёжзийклмнопрстуфхцчшщъыьэюя",
                    alphabetUp = "АБВГДЕЁЖЗИЙКЛМНОПРСТУФЧЦЧШЩЪЫЬЭЮЯ";

                //Take values
                var vm = this,
                    string = vm.msg_de,
                    shiftAmount = vm.shift_de * -1,
                    output_de = vm.output_de = "";

                //Shift alphabet
                var shiftedAlphabetLower = alphabetLower.slice(shiftAmount) + alphabetLower.slice(0, shiftAmount) + otherCharacters,
                    shiftedAlphabetUp = alphabetUp.slice(shiftAmount) + alphabetUp.slice(0, shiftAmount) + otherCharacters;

                alphabetLower += otherCharacters;
                alphabetUp += otherCharacters;

                //Comparison simbols
                function simbol(symb, arr) {
                    for (var i = 0; i < arr.length; i++) {
                        if (symb === arr[i]) {
                            return true;
                        }
                    }
                }

                //Decrypt message
                for (var i = 0; i < string.length; i++) {
                    if (simbol(string[i], alphabetLower)) {
                        var number = alphabetLower.indexOf(string[i]);
                        output_de += shiftedAlphabetLower[number];
                    } else {
                        var number = alphabetUp.indexOf(string[i]);
                        output_de += shiftedAlphabetUp[number];
                    }
                }

                vm.output_de = output_de
            }
        }
    }
</script>

<template>
    <div class="wrapper">
        <h1>Шифратор Цезаря</h1>
        <div>
            <br />
            <span>Зашифровать</span>
            <input type="text" v-model="msg_en" placeholder="Введите текст" />
            <span>, смещение равно</span>
            <input type="text" v-model="shift_en" placeholder="Введите число" />
            <button v-if="msg_en !=''" @click="encrypt()">Зашифровать</button>
            <button disabled v-else>Введите текст</button>
        </div>
        <div class="output">
            <div class="child_output" style="width{0}">{{ output_en }}</div>
        </div>
        <div>
            <br />
            <span>Расшифровать</span>
            <input type="text" v-model="msg_de" placeholder="Введите текст" />
            <span>, смещение равно</span>
            <input type="text" v-model="shift_de" placeholder="Введите число" />
            <button v-if="msg_de !=''" @click="decrypt()">Расшифровать</button>
            <button disabled v-else>Введите текст</button>
        </div>
        <div class="output">
            <div class="child_output" style="width{0}">{{ output_de }}</div>
        </div>
    </div>
</template>

<style scoped>
    .wrapper {
        width: 900px;
        height: 300px;
        border-radius: 50px;
        background: #1f0f24;
        padding: 20px;
        text-align: center;
        color: #fff;
    }

    .wrapper h1 {
        margin-top: 50px;
    }

    .wrapper p {
        margin-top: 20px;
    }

    .wrapper input {
        margin-top: 30px;
        background: transparent;
        border: 0;
        border-bottom: 2px solid #110813;
        color: #fcfcfc;
        font-size: 14px;
        padding: 5px 8px;
        outline: none;
    }

    .wrapper input:focus {
        border-bottom-color: #6e2d7d;
    }

    .wrapper button{
        background: #e3bc4b;
        color: #fff;
        border-radius: 10px;
        border: 2px solid #b99935;
        padding: 10px 15px;
        margin-left: 20px;
        cursor: pointer;
        transition: transform 500ms ease;
    }

    .wrapper button:hover {
        transform: scale(1.1) translateY(-5px);
    }

    .wrapper button:disabled {
        background: #746027;
        cursor: not-allowed;
    }

    .wrapper .output {
        display: flex;
        justify-content: center;
    }

    .wrapper .output .child_output{
        height: 0;
    }
</style>
