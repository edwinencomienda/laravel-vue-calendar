<template>
    <div class="container">
        <div class="row">
            <div class="col-md-2">
                <div id='external-events'>
                    <p>
                        <strong>Draggable Events</strong>
                    </p>
                    <div v-for="subject in subjects" class='fc-event' :subject-id="subject.id">{{ subject.name }}</div>
                    <p>
                        <input type='checkbox' id='drop-remove' />
                        <label for='drop-remove'>remove after drop</label>
                    </p>
                </div>
            </div>
            <div class="col-md-10">
                <full-calendar 
                locale="en"
                :events="events"
                :config="config"
                @event-receive="onEventReceive"
                @event-drop="onEventDrop"
                />
            </div>
        </div>
    </div>
</template>

<script>
import { FullCalendar } from 'vue-full-calendar'
import 'fullcalendar/dist/fullcalendar.css'
import 'jquery-ui-dist/jquery-ui'

export default {
    components: {
        FullCalendar
    },

    data () {
        return {
            config: {
                header: {
                    left:   'prev,next today',
                    center: 'title',
                    right:  'month,agendaWeek,agendaDay'
                },
                editable: true,
                droppable: true,
                selectable: true,
                defaultView: 'agendaWeek',
            },

            events: [],

            subjects: [
                {
                    name: 'Math',
                    id: 1
                },
                {
                    name: 'English',
                    id: 2
                }
            ]
        }
    },

    mounted () {
        $('#external-events .fc-event').each(function() {

            // store data so the calendar knows to render an event upon drop
            $(this).data('event', {
                title: $.trim($(this).text()), // use the element's text as the event title
                stick: true, // maintain when user navigates (see docs on the renderEvent method)
                subjectId: $(this).attr('subject-id')
            });

            // make the event draggable using jQuery UI
            $(this).draggable({
                zIndex: 999,
                revert: true,      // will cause the event to go back to its
                revertDuration: 0  //  original position after the drag
            });

        });
    },

    methods: {
        /**
         * When drag from sidebar list to calendar
         * You can call ajax here to your endpoint
         */
        onEventReceive (event, ui) {
            console.log(event.subjectId, event)
        },

        /**
         * When drag from calendar to calendar
         * You can call ajax here to your endpoint
         */
        onEventDrop (event, ui) {
            console.log(event.subjectId, event)
        },
    }
}
</script>
