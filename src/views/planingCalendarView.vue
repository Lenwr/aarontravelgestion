<script setup>
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import listPlugin from '@fullcalendar/list'
import interactionPlugin from '@fullcalendar/interaction'
import {reactive, ref, watch} from "vue";
import FullCalendar from "@fullcalendar/vue3";
import {collection} from "firebase/firestore";
import useEvents from "../components/useEvents.js";
import {useFirestore} from "vuefire";

const {getEvents , createEvent , setEvents , updateEvent , deleteEvent} =  useEvents()
const id = ref(10)
const db = useFirestore()
const eventsCollection = collection(db, 'events');
const showModal = ref(false);

let todayStr = new Date().toISOString().replace(/T.*$/, '') // YYYY-MM-DD of today




const options = reactive({
  plugins: [dayGridPlugin , timeGridPlugin , listPlugin , interactionPlugin] ,
  initialView :  'dayGridMonth' ,
  headerToolbar : {
    left : 'prev,next today',
    center : 'title',
    right: 'dayGridMonth,dayGridWeek,listDay'
  },
  editable : true ,
  selectable : true ,
  initialEvents: getEvents.value,
  eventsSet: getEvents.value,
  select: (arg) => {
   const cal = arg.view.calendar
    cal.unselect()
    cal.select({
       title : prompt('Please enter a new title for your event'),
    })
    cal.addEvent({
    })
  },
  eventClick: (arg)=> {
    console.log(arg)
  },
  events: [],
  eventAdd: (arg) => {

  },
});

options.events = getEvents.value
watch(getEvents , () =>{
  options.events = getEvents.value
})


</script>





<template>
<div class="pt-8 pb-[20%] px-1">
      <FullCalendar
          class='text-black
mobile:text-[0.5em]'
          :options='options'
      >
      </FullCalendar>
</div>
</template>
