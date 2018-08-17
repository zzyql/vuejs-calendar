<template>
    <div :class="classObejct" @click="captureClick">
        {{ day.format('D') }}
        <ul class="event-list">
            <li v-for="event in events">{{ event.description }}</li>
        </ul>
    </div>
</template>


<script>
export default {
    props:['day',],
    computed: {
        events() {
            let mockData = [
                { description: 'Random Event1', date: this.$moment('2018-08-06', 'YYYY-MM-DD')},
                { description: 'Random Event2', date: this.$moment('2018-07-31', 'YYYY-MM-DD')},
                { description: 'Random Event3', date: this.$moment('2018-08-31', 'YYYY-MM-DD')},
            ];
            return this.$store.state.events.filter(event => event.date.isSame(this.day, 'day'));
        },
        classObejct() {
            let today = this.day.isSame(this.$moment(), 'day');
            let eventFormActive = this.$store.state.eventFormActive;
            let eventFormDate = this.$store.state.eventFormDate;
            return {
                day: true,
                today,
                past: this.day.isSameOrBefore(this.$moment(), 'day') && !today,
                active: eventFormDate.isSame(this.day, 'day') && eventFormActive,
            }
        }
    },
    methods: {
        captureClick(event) {
            this.$store.commit('eventFormPos', { x: event.clientX, y: event.clientY});
            this.$store.commit('eventFormActive', true);
            this.$store.commit('eventFormDate', this.day);
        }
    }

}
</script>
