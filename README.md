# timeleftjs

## Description

Show you the actual amount of useful time you have left.

### Features

- subtracts time you have committed for other things. Examples:
  - breaks
  - meals
  - meetings
  - travel time
  - other commitments
  - "gray" time - time where things are going on but you could skip if you had to
- Pull from external calendar(s), e.g.
  - Google Calendar
  - iCal
  - Outlook
- Settings:
  - What time is the "end" of your day?
  - When are your breaks?
  - What commitments do you have today?
  - What is the max time to show?
- **NOTE! AS BREAKS AND MEETINGS PASS, THEY NO LONGER NEED TO BE SUBTRACTED FROM YOUR AVAILABLE TIME!!**

### Key Technical Issues

- A) How to get the reserved time from the person's calendar and actually multiple calendars?
  - How technically to code this
  - Authorization
  - Storing the person's authorization
- B) How to store these events. And when do you update your copy?
- C) Dealing with partial time (see below)
- D) Is it draining a lot of resources to have it update every second? Change it to every minute? Or not a big deal? Or change it to every minute anyway because I don't personally need it?
- E) I think there was something else
- **NOTE! AS BREAKS AND MEETINGS PASS, THEY NO LONGER NEED TO BE SUBTRACTED FROM YOUR AVAILABLE TIME!!**
  - So do we need to know the start/end times of each thing that is being removed?
  - How do we remove part of the time? e.g. if it is halfway through lunch, only 30 minutes have passed but not the whole thing?
- If I generate a new block for each hour of time, plus one for partial hour, it will illustrate number of hours
  - or do this with background? SVG background? But I'd want it to scale

### Design

- Create rough designs using tldraw, excalidraw, figma, pen & paper
- Total time is out of the "current period"
  - could have this shrink down to a smaller base period like an hour as the time left shrinks
- Break up the block into hours visually. (maybe quarter-hours also?)
- What happens when time left goes negative? Option: could go below the zero mark?
  - OR?? should it count up, and be adding to "overtime"? Or subtract from "overtime"? We currently don't show "overtime" so I would need to design for that.
- Future/Maybe: show different categories of time in different colors, for example, blue for clear, red for meetings, purple for breaks that might be optional, and have these stacked, so not just your free time
- DURING EVENT: If we are currently in the middle of an event, and the time left isn't decreasing, show something to indicate this. Text for "in a meeting" or an icon.
- DURING "Current Event": Also, could countdown the timeleft until end of "current event"
- NO "Current Event": Add countdown of timeleft until start of next event
- Each hour is a "battery" and you have the "current battery" (1 hr), then separate "row of batteries" for number of additional hours
- UI: render HTML another way? Template engine? Put into HTML and just update parts?
- UI: colors for different periods of the day. dark at night, yellow/orange before.
- UI: more height 50%. How make it proportional to viewport?
- UI: less space above
- UI: timer not move.
- UI: multiple timers side-by-side? early, prime, late, after, and they all go down to nothing?
- Different layouts?
  - Just available time
  - Available time plus other types of time
  - Available time above a line, other types below the line
- What if able to show a week? How much time for each day for 7 days?

### Coding

- CODE: Refactor code... templating? Svelte? T3?
- CODE: Refactor code... JSDoc? take out, leave in?

- AUTH: Google API: How qualify to verify, for non-experimental access? e.g. privacy policy. Check my other project to see if I did it for that. - what was the other project?
- FIXME: login to thinkabout, by github/discord, to fix???
- REFACTOR: how to organize the JS into modules, files?
- TECH: AUTH: How to remember me and not need full login
- TECH: AUTH: Use PassportJS? check other project to see how that's done. probably a different project
- User Settings: Calendar: Pick the calendar(s), or do all calendars? Also, put FocusMate onto different default calendar

### Date/Time functions

- Temporal: temporal-polyfill
- now as ZonedDateTimeISO
- nowTime is PlainTime - from now (so not a new time)
- nowTimeZone

### User Settings

- would have what?
- [ ] Settings: Basic:
  - [ ] start/end of day... periods start/end
  - [ ] Which calendar to use for events (calendars?)

## Plan

- v1: display as countdown timer
- v2: display graphically:
  - as bar, going lower/shorter as time progresses to visualize the time diminishing
  - circle? although I don't like that as much
- Data storage?
  - LocalStorage
  - IndexedDB (only if you need accessible database)
  - SQLite
  - Flat files
  - MongoDB (or Firebase)
  - SQL (host?)
- Auth/login?
  - need logins for calendars! So Google API login.
  - what about iCal / Apple Calendar login?
- Can use the todo-mvc-auth to save user settings to MongoDB (uses EJS though) (and has PassportJS)
- Date Time formats
  - NOTE: USING ISO 8601 - GOOGLE CALENDAR API FORMAT (TO START)
  - .ics - TBD
  - JavaScript Temporal API (temporal-polyfill)
- Plain JavaScript or React?
  - React would make it easier to switch between layouts
  - and to switch to a settings panel

### Plan: MVP

- countdown timer
- hard-code values:
  - start of day: default: 9am
  - end of day: default: 9pm
  - total break time default: 4.0
    - lunch: 1
    - dinner: 2
    - breaks: 1
  - total scheduled time: default: 0.0
- only one user
- no calendar login

## TODO - MVP 0.1

- [x] Display countdown
- [x] Hard-code settings
  - [x] start of day
  - [x] end of day
  - [x] total break time
  - [x] total scheduled time
- [ ] Smaller steps:
  - [ ] EXAMPLE EVENTS
    - [ ] "TOO EARLY" - start and end are both before period start - exclude
    - [ ] "TOO LATE" - start and end are both after period end - exclude
    - [ ] "DURING" - start is after period start; end is before period end - subtract
    - [ ] "DURING FUTURE" - simplest: start and end are both after now (in the future) and both after period start and before period end (and do not overlap)
    - [ ] "DURING PAST" - start and end are both in this period before now
    - [ ] "DURING OVERLAP START"
    - [ ] "DURING OVERLAP END"
    - CASES involving "NOW"
    - [ ] "DURING OVERLAP NOW"
    - [ ] - start before now
    - [ ] - start before now; end after period
  - [ ] SPECIAL CASES
    - [ ] If EventEnd has passed, do not subtract EventTime
    - [ ] what if Event has already started - and not passed
      - subtract: now - eventStart from totalTime
    - [ ] _WHAT IF TWO EVENTS OVERLAP?_
- USING GOOGLE CALENDAR API FORMAT (TO START)

## TODO - MVP 0.2

- [ ] Display current date and time (optional)
- [ ] Form to enter settings
- [ ] Settings: Basic:
  - [ ] end of day
  - [ ] total break time
  - [ ] total scheduled time

## TODO - v2

- [ ] Show number of hours, to the side (or top if it's horizontal)
- [ ] Visual display: Bar with size decreasing
- [ ] Bar height = total time (starting when??)

# Future/Maybe

- Tech: Standalone app (Electron? React Native?)
- Be able to have as a component on a separate dashboard

# Log

### TODO

#### Checklist

- [ ] check console
- [ ] Today's date being used?
- [ ] current periodStart correct?
- [ ] current periodEnd correct?
- [ ] duration time now until periodEnd? DISPLAY IT
