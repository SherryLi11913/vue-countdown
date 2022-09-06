<template>
    <div class="countdown-container" v-show="timerLoaded">
        <h1 v-if="dateExpired">Time is Up</h1>
        <h1 v-else>Buy Now</h1>

        <div class="timer-container">
            <section class="time-section">
                <div class="time">
                    {{ formatTime(days) }}
                </div>
                <div class="time-unit">
                    days
                </div>
            </section>

            <span class="time-separator">:</span>

            <section class="time-section">
                <div class="time">
                    {{ formatTime(hours) }}
                </div>
                <div class="time-unit">
                    hours
                </div>
            </section>

            <span class="time-separator">:</span>

            <section class="time-section">
                <div class="time">
                    {{ formatTime(minutes) }}
                </div>
                <div class="time-unit">
                    minutes
                </div>
            </section>

            <span class="time-separator">:</span>

            <section class="time-section">
                <div class="time">
                    {{ formatTime(seconds) }}
                </div>
                <div class="time-unit">
                    seconds
                </div>
            </section>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CounterComponent',
    props: {
        year: {
            type: Number,
            required: true,
        },
        month: {
            type: Number,
            required: true,
        },
        day: {
            type: Number,
            required: true,
        },
        hour: {
            type: Number,
            default: 0,
        },
        minute: {
            type: Number,
            default: 0,
        },
        second: {
            type: Number,
            default: 0,
        },
        millisecond: {
            type: Number,
            default: 0,
        },
    },
    data() {
        return {
            days: 0,
            hours: 0,
            minutes: 0,
            seconds: 0,
            timerLoaded: false,
            dateExpired: false,
        };
    },
    computed: {
        _seconds() {
            return 1000;
        },
        _minutes() {
            return this._seconds * 60;
        },
        _hours() {
            return this._minutes * 60;
        },
        _days() {
            return this._hours * 24;
        },
        end() {
            return new Date(this.year, this.month, this.day, this.hour, this.minute, this.second, this.millisecond);
        },
    },
    mounted() {
        this.countdown();
    },
    methods: {
        countdown() {
            const timer = setInterval(() => {
                const distance = this.end.getTime() - Date.now();

                if (distance <= 0) {
                    this.dateExpired = true;
                    this.resetTimer();
                    this.timerLoaded = true;
                    clearInterval(timer);
                    return;
                }

                this.days = Math.floor(distance / this._days);
                this.hours = Math.floor((distance % this._days) / this._hours);
                this.minutes = Math.floor((distance % this._hours) / this._minutes);
                this.seconds = Math.floor((distance % this._minutes) / this._seconds);
                this.timerLoaded = true;
            }, 1000);
        },
        formatTime(time) {
            return `${time < 10 ? '0' : ''}${time}`;
        },
        resetTimer() {
            this.days = 0;
            this.hours = 0;
            this.minutes = 0;
            this.seconds = 0;
        }
    },
}
</script>

<style scoped>
.countdown-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.timer-container {
    width: 100%;
    display: flex;
    justify-content: center;
}

.time-section {
    width: 15%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.time {
    font-size: 40px;
    font-weight: 700;
}

.time-unit {
    font-size: 20px;
}

.time-separator {
    font-size: 30px;
}
</style>