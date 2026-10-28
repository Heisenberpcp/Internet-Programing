Answer
----

1. In EventList, what goes wrong if you use the array index as :key and the list gets filtered while a 
card has local state (for example an open details panel)? 
> Vue may reuse a card instance for a different event after filtering, so its local state (such as an open details panel) can appear on the wrong event. A stable key like `event.id` keeps each card associated with the correct event.
2. Why is visible a computed and not a function called from the template? What is the difference 
when you type in the search box? 
> Because `visible` is derived from the events and filter values. A computed value is cached and recalculated when its dependencies change, typing in the search box updates the search ref, so Vue recalculates the filtered list. A template function would run each time the component renders.
3. EventCard emits book instead of changing event.booked. Give two reasons. 
> The parent owns the event data, so it remains the single source of truth and can validate the booking (for example, whether seats remain). The parent can also update related state such as `myBookings`; the card only needs to emit the user's intent.
4. You write const { booked } = reactive(event); booked++. The page does not change. 
Why?
> Because it is  Destructuring a primitive property from a reactive object creates a local value that is no longer linked to the reactive property. Incrementing it does not change `event.booked`, so Vue has no reactive change to render. Use `event.booked++` or `toRef(event, "booked")`.
5. Where does myBookings live, and why not in EventCard or EventList?
> `myBookings` lives in `App.vue`, which owns the shared booking state and booking count. `EventCard` and `EventList` display events and emit booking events; they should not own this application-level state.