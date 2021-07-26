<template>
    <div>
        <v-dialog
            v-model="dialog"
            fullscreen
            hide-overlay
            persistent
            no-click-animation
        >
            <v-card>
                <v-toolbar color="primary">
                    <v-spacer></v-spacer>
                    <v-toolbar-items>
                        <v-btn tile color="primary" dark @click="close(true)">
                            Close
                            <v-icon>close</v-icon>
                        </v-btn>
                    </v-toolbar-items>
                </v-toolbar>
                <v-form
                    ><v-card class="mx-auto" max-width="1000">
                        <v-card-text>
                            <v-row class="text-center">
                                <v-col cols="12">
                                    <v-card>
                                        <v-row>
                                            <v-col cols="3"> </v-col>
                                            <v-col cols="6">
                                                <h3
                                                    :class="
                                                        'display-3 font-weight-bold'
                                                    "
                                                >
                                                    Is next roll higher or
                                                    lower?
                                                </h3>
                                            </v-col>
                                            <v-col cols="3"> </v-col>
                                        </v-row>
                                        <v-row
                                            class="text-center"
                                            justify="center"
                                            align="center"
                                        >
                                            <v-col cols="3">
                                                <v-btn
                                                    rounded
                                                    color="primary"
                                                    dark
                                                    x-large
                                                    @click="roll(isLower)"
                                                    ><span class="mr-2"
                                                        >Lower</span
                                                    ><v-icon
                                                        >arrow_downward</v-icon
                                                    ></v-btn
                                                ></v-col
                                            >
                                            <v-col cols="6">
                                                <v-img
                                                    :src="diceUrl"
                                                    class="my-3"
                                                    contain
                                                    height="300"
                                                /> </v-col
                                            ><v-col cols="3">
                                                <v-btn
                                                    rounded
                                                    color="primary"
                                                    dark
                                                    x-large
                                                    @click="roll(!isLower)"
                                                    ><span class="mr-2"
                                                        >Higher</span
                                                    ><v-icon
                                                        >arrow_upward</v-icon
                                                    ></v-btn
                                                ></v-col
                                            >
                                        </v-row>

                                        <v-row
                                            v-if="classText != ''"
                                            class="text-center"
                                            justify="center"
                                            align="center"
                                        >
                                            <v-col cols="6" class="mb-4">
                                                <h3
                                                    :class="
                                                        'display-3 font-weight-bold ' +
                                                            classText
                                                    "
                                                >
                                                    You are
                                                    {{ result }}
                                                </h3>
                                            </v-col>
                                        </v-row>

                                        <v-row
                                            class="text-center"
                                            justify="center"
                                            align="center"
                                        >
                                            <v-col cols="6" class="mb-4">
                                                <v-card>
                                                    <v-card-text>
                                                        <h2
                                                            class="display-2 font-weight-bold mb-3"
                                                        >
                                                            Score
                                                        </h2>

                                                        <h1
                                                            class="subheading font-weight-bold"
                                                        >
                                                            {{ score }} / 3
                                                        </h1>
                                                    </v-card-text>
                                                </v-card>
                                            </v-col>
                                            <v-col cols="6" class="mb-4">
                                                <v-card>
                                                    <v-card-text>
                                                        <h2
                                                            class="display-2 font-weight-bold mb-3"
                                                        >
                                                            Round
                                                        </h2>

                                                        <h1
                                                            class="subheading font-weight-bold"
                                                        >
                                                            {{ round }} / 30
                                                        </h1>
                                                    </v-card-text>
                                                </v-card>
                                            </v-col>
                                        </v-row>
                                        <v-toolbar color="primary" dark>
                                            <v-toolbar-title
                                                >History</v-toolbar-title
                                            >
                                        </v-toolbar>
                                        <v-card
                                            v-for="(historyItem,
                                            index) in historyItems"
                                            :key="index"
                                            class="mt-2"
                                            tile
                                            elevation="2"
                                        >
                                            <v-card-text>
                                                <v-row
                                                    class="px-3"
                                                    justify="space-between"
                                                >
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Round
                                                            <span
                                                                class="font-weight-bold"
                                                                >{{
                                                                    historyItem.round
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Previous Roll:
                                                            <span
                                                                class="font-weight-bold"
                                                                >{{
                                                                    historyItem.previousRoll
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Decision:
                                                            <span
                                                                class="font-weight-bold"
                                                                >{{
                                                                    historyItem.decision
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Next Roll:
                                                            <span
                                                                class="font-weight-bold"
                                                                >{{
                                                                    historyItem.nextRoll
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Result:
                                                            <span
                                                                :class="
                                                                    'font-weight-bold' +
                                                                        'color: ' +
                                                                        historyItem
                                                                            .result
                                                                            .color +
                                                                        '--text'
                                                                "
                                                                >{{
                                                                    historyItem
                                                                        .result
                                                                        .result
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                    <v-col class="col-auto">
                                                        <div>
                                                            Score:
                                                            <span
                                                                class="font-weight-bold"
                                                                >{{
                                                                    historyItem.score
                                                                }}</span
                                                            >
                                                        </div>
                                                    </v-col>
                                                </v-row>
                                            </v-card-text>
                                        </v-card>
                                    </v-card>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card></v-form
                >
            </v-card>
            <v-dialog
                v-model="finishDialog"
                hide-overlay
                persistent
                no-click-animation
            >
                <template>
                    <v-card>
                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col cols="12">
                                        <h1 class="subheading font-weight-bold">
                                            Congratulations!
                                        </h1>
                                    </v-col>
                                    <v-col cols="12">
                                        <h2
                                            class="display-2 font-weight-bold mb-3"
                                        >
                                            Your score is {{ score }}
                                        </h2>
                                    </v-col>
                                    <v-col cols="3">
                                        <v-btn
                                            rounded
                                            color="primary"
                                            dark
                                            @click="close(false)"
                                            ><span class="mr-2"
                                                >Exit to menu</span
                                            ><v-icon>exit</v-icon></v-btn
                                        ></v-col
                                    >
                                </v-row>
                            </v-container>
                        </v-card-text>
                    </v-card>
                </template>
            </v-dialog>
        </v-dialog>
    </div>
</template>

<script>
export default {
    props: {
        resumeGame: {
            type: Boolean,
            default: false
        }
    },
    name: "Rollview",

    data: () => ({
        dialog: false,
        dice: {},
        diceUrl: "",
        nextRoll: 0,
        previousRoll: 0,
        round: 1,
        score: 0,
        isLower: true,
        finishDialog: false,
        historyItems: [],
        decision: "",
        result: "",
        classText: "",
        url: "http://roll.diceapi.com/json/d6"
    }),
    watch: {
        async dialog() {
            if (this.dialog) {
                if (!this.resumeGame) {
                    await this.fetchData();
                    this.previousRoll = this.dice.value;
                    this.nextRoll = 0;
                    this.round = 1;
                    this.score = 0;
                    this.classText = "";
                } else {
                    var keys = Object.keys(localStorage);
                    var i = keys.length;
                    while (i-- && i >= 0) {
                        this.historyItems.push(
                            JSON.parse(localStorage.getItem(keys[i]))
                        );
                        this.historyItems.sort((a, b) =>
                            a.round < b.round ? -1 : a.round > b.round ? 1 : 0
                        );
                    }
                    this.diceIconGet(
                        this.historyItems[this.historyItems.length - 1].nextRoll
                    );
                }
            }
        }
    },
    methods: {
        open() {
            this.dialog = true;
        },
        close(resumeAvailable) {
            this.dialog = false;
            this.$emit("close", resumeAvailable);
            this.historyItems = [];

            if (!resumeAvailable) {
                this.dice = {};
                this.diceUrl = "";
                this.nextRoll = 0;
                this.previousRoll = 0;
                this.round = 1;
                this.score = 0;
                localStorage.clear();
            }
        },
        async fetchData() {
            // do wyrzucenia generateNumber() oraz modyfikacja reszty usług
            await this.generateRoll();
            this.diceIconGet(this.nextRoll);
        },
        diceIconGet(number) {
            this.diceUrl =
                "http://roll.diceapi.com/images/poorly-drawn/d6/" +
                number +
                ".png";
        },
        async generateRoll() {
            await fetch(this.url, {})
                .then(response => {
                    if (response.ok) {
                        return response.json();
                    } else {
                        alert(
                            "Server returned " +
                                response.status +
                                " : " +
                                response.statusText
                        );
                    }
                })
                .then(response => {
                    this.dice = response.dice[0];
                })
                .catch(err => {
                    console.log(err);
                });
            this.nextRoll = this.dice.value;
            if (this.nextRoll == this.previousRoll) {
                this.generateRoll();
            }
        },
        roll(isLower) {
            if (this.round <= 30) {
                this.decision = isLower ? "Lower" : "Higher";
                if (isLower) {
                    this.fetchData();
                    this.checkRoll(this.previousRoll, this.nextRoll, true);
                } else {
                    this.fetchData();
                    this.checkRoll(this.previousRoll, this.nextRoll, false);
                }
                this.localStorageSaveData();
                this.previousRoll = this.nextRoll;
                if (this.round <= 30) {
                    this.round++;
                }
                if (this.round == 31) {
                    this.finishDialog = true;
                }
            }
        },
        checkRoll(previous, current, isLower) {
            this.result = "Wrong";
            if (previous < current && !isLower) {
                this.score += 0.1;
                this.result = "Correct";
            } else if (previous > current && isLower) {
                this.score += 0.1;
                this.result = "Correct";
            }
            if (this.result == "Correct") {
                this.classText = "color: green--text";
            } else {
                this.classText = "color: red--text";
            }
            this.score = parseFloat(this.score.toFixed(1));
        },
        localStorageSaveData() {
            var input = {
                round: this.round,
                score: this.score,
                previousRoll: this.previousRoll,
                nextRoll: this.nextRoll,
                decision: this.decision,
                result: {
                    result: this.result,
                    color: this.result == "Correct" ? "green" : "red"
                }
            };
            localStorage.setItem(this.round, JSON.stringify(input));
            this.historyUpdate(JSON.parse(localStorage.getItem(this.round)));
        },
        historyUpdate(item) {
            this.historyItems.push(item);
        }
    }
};
</script>
