
# 🕐 Java 8 Date and Time API

> **The Java 8 Date and Time API (`java.time`) provides immutable, thread-safe, and clearer classes for working with dates, times, durations, periods, instants, and time zones.**

---

# 📑 Table of Contents

- [1. Why Was a New Date and Time API Needed?](#1-why-was-a-new-date-and-time-api-needed)
- [2. Old Date and Time APIs](#2-old-date-and-time-apis)
- [3. Java 8 Date and Time API](#3-java-8-date-and-time-api)
- [4. Important Classes](#4-important-classes)
- [5. LocalDate](#5-localdate)
- [6. Creating LocalDate](#6-creating-localdate)
- [7. Getting Date Information](#7-getting-date-information)
- [8. Modifying LocalDate](#8-modifying-localdate)
- [9. LocalTime](#9-localtime)
- [10. Creating LocalTime](#10-creating-localtime)
- [11. Modifying LocalTime](#11-modifying-localtime)
- [12. LocalDateTime](#12-localdatetime)
- [13. Instant](#13-instant)
- [14. ZonedDateTime](#14-zoneddatetime)
- [15. ZoneId](#15-zoneid)
- [16. OffsetDateTime](#16-offsetdatetime)
- [17. Period](#17-period)
- [18. Duration](#18-duration)
- [19. Period vs Duration](#19-period-vs-duration)
- [20. Comparing Dates and Times](#20-comparing-dates-and-times)
- [21. Formatting Dates](#21-formatting-dates)
- [22. Parsing Dates](#22-parsing-dates)
- [23. DateTimeFormatter](#23-datetimeformatter)
- [24. ChronoUnit](#24-chronounit)
- [25. ChronoField](#25-chronofield)
- [26. Leap Year](#26-leap-year)
- [27. Date Arithmetic](#27-date-arithmetic)
- [28. Immutability](#28-immutability)
- [29. Thread Safety](#29-thread-safety)
- [30. Legacy Date Conversion](#30-legacy-date-conversion)
- [31. Common Mistakes](#31-common-mistakes)
- [32. Interview Questions](#32-interview-questions)
- [33. 30-Second Interview Answer](#33-30-second-interview-answer)
- [34. Cheat Sheet](#34-cheat-sheet)
- [35. Final Mental Model](#35-final-mental-model)

---

# 1. Why Was a New Date and Time API Needed?

Before Java 8, Java commonly used:

```text
java.util.Date
java.util.Calendar
java.text.SimpleDateFormat
```

These APIs had several design problems.

Important issues included:

```text
Mutable objects
Confusing APIs
Poor separation of date and time
Thread-safety problems with SimpleDateFormat
Difficult timezone handling
Less readable date calculations
```

Java 8 introduced:

```text
java.time
```

with a much cleaner design.

---

# 2. Old Date and Time APIs

## java.util.Date

`Date` represented a point in time, but its API contained several confusing and outdated methods.

Example:

```java
Date date = new Date();

System.out.println(date);
```

---

## Calendar

`Calendar` provided more operations than `Date`, but its API was verbose and mutable.

Example:

```java
Calendar calendar =
    Calendar.getInstance();

System.out.println(
    calendar.get(Calendar.YEAR)
);
```

---

## SimpleDateFormat

`SimpleDateFormat` was commonly used for formatting and parsing.

Example:

```java
SimpleDateFormat format =
    new SimpleDateFormat("dd-MM-yyyy");

String result =
    format.format(new Date());

System.out.println(result);
```

A major problem is that `SimpleDateFormat` is not thread-safe.

---

# 3. Java 8 Date and Time API

The new API is primarily located in:

```text
java.time
```

Important classes include:

```text
LocalDate
LocalTime
LocalDateTime
Instant
ZonedDateTime
OffsetDateTime
ZoneId
Period
Duration
DateTimeFormatter
```

Conceptually:

```text
                 java.time
                     |
       +-------------+-------------+
       |             |             |
      Date          Time       Date + Time
       |             |             |
 LocalDate       LocalTime    LocalDateTime
       |
       +-------------------------+
                                 |
                         ZonedDateTime
                                 |
                              ZoneId

       +-------------------------+
       |
   Amount of time
       |
   +---+---+
   |       |
Period  Duration
```

---

# 4. Important Classes

| Class | Represents |
|---|---|
| `LocalDate` | Date without time/timezone |
| `LocalTime` | Time without date/timezone |
| `LocalDateTime` | Date + time without timezone |
| `Instant` | A point on the UTC timeline |
| `ZonedDateTime` | Date + time + timezone |
| `OffsetDateTime` | Date + time + UTC offset |
| `ZoneId` | Time-zone identifier |
| `Period` | Date-based amount |
| `Duration` | Time-based amount |
| `DateTimeFormatter` | Formatting/parsing |
| `ChronoUnit` | Date/time units |
| `ChronoField` | Individual date/time fields |

---

# 5. LocalDate

`LocalDate` represents:

```text
Year + Month + Day
```

It does **not** contain:

```text
Time
Timezone
UTC offset
```

Example:

```java
LocalDate today =
    LocalDate.now();

System.out.println(today);
```

Possible output:

```text
2026-09-28
```

---

# 6. Creating LocalDate

## Current Date

```java
LocalDate today =
    LocalDate.now();
```

---

## Specific Date

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);
```

---

## Using Month Enum

```java
LocalDate date =
    LocalDate.of(
        2026,
        Month.SEPTEMBER,
        28
    );
```

---

## Parsing ISO Date

```java
LocalDate date =
    LocalDate.parse("2026-09-28");
```

The default ISO format is:

```text
yyyy-MM-dd
```

---

# 7. Getting Date Information

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);
```

Get year:

```java
int year =
    date.getYear();
```

Get month:

```java
Month month =
    date.getMonth();
```

Get month number:

```java
int month =
    date.getMonthValue();
```

Get day of month:

```java
int day =
    date.getDayOfMonth();
```

Get day of year:

```java
int dayOfYear =
    date.getDayOfYear();
```

Get day of week:

```java
DayOfWeek day =
    date.getDayOfWeek();
```

---

# 8. Modifying LocalDate

`LocalDate` is immutable.

Therefore, methods such as:

```text
plusDays()
minusDays()
plusMonths()
minusMonths()
```

return a new object.

Example:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalDate nextDate =
    date.plusDays(10);

System.out.println(date);
System.out.println(nextDate);
```

The original `date` remains unchanged.

---

## Add Months

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalDate result =
    date.plusMonths(2);
```

---

## Subtract Days

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalDate result =
    date.minusDays(5);
```

---

## With Methods

You can directly replace a component.

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalDate result =
    date.withYear(2030);
```

---

# 9. LocalTime

`LocalTime` represents:

```text
Hour + Minute + Second + Nanosecond
```

It does not contain:

```text
Date
Timezone
UTC offset
```

Example:

```java
LocalTime time =
    LocalTime.now();

System.out.println(time);
```

Possible output:

```text
14:30:25.123456789
```

---

# 10. Creating LocalTime

## Current Time

```java
LocalTime time =
    LocalTime.now();
```

---

## Specific Time

```java
LocalTime time =
    LocalTime.of(14, 30);
```

---

## With Seconds

```java
LocalTime time =
    LocalTime.of(
        14,
        30,
        45
    );
```

---

## Parsing

```java
LocalTime time =
    LocalTime.parse("14:30:45");
```

---

## Getting Components

```java
LocalTime time =
    LocalTime.of(
        14,
        30,
        45
    );
```

Hour:

```java
int hour =
    time.getHour();
```

Minute:

```java
int minute =
    time.getMinute();
```

Second:

```java
int second =
    time.getSecond();
```

Nano:

```java
int nano =
    time.getNano();
```

---

# 11. Modifying LocalTime

```java
LocalTime time =
    LocalTime.of(14, 30);
```

Add hours:

```java
LocalTime result =
    time.plusHours(2);
```

Add minutes:

```java
LocalTime result =
    time.plusMinutes(30);
```

Subtract minutes:

```java
LocalTime result =
    time.minusMinutes(15);
```

Again, the original object is not changed.

---

# 12. LocalDateTime

`LocalDateTime` combines:

```text
LocalDate
+
LocalTime
```

It does not contain timezone information.

Example:

```java
LocalDateTime now =
    LocalDateTime.now();

System.out.println(now);
```

Possible output:

```text
2026-09-28T14:30:25
```

---

## Creating Specific Date-Time

```java
LocalDateTime dateTime =
    LocalDateTime.of(
        2026,
        9,
        28,
        14,
        30
    );
```

---

## Combining Date and Time

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalTime time =
    LocalTime.of(14, 30);

LocalDateTime dateTime =
    LocalDateTime.of(
        date,
        time
    );
```

---

## Important

`LocalDateTime` does **not** represent a globally unique moment.

For example:

```text
2026-09-28 14:30
```

could occur simultaneously in many time zones.

If you need timezone information, use:

```text
ZonedDateTime
```

or an offset-aware type.

---

# 13. Instant

`Instant` represents a point on the UTC timeline.

Think of it as:

```text
A precise moment
```

Example:

```java
Instant now =
    Instant.now();

System.out.println(now);
```

Possible output:

```text
2026-09-28T09:00:00Z
```

`Z` means:

```text
UTC
```

---

## When to Use Instant

Use `Instant` when you care about:

```text
Event timestamps
Database timestamps
Logs
Server-to-server communication
Measuring points on the global timeline
```

Example:

```java
Instant start =
    Instant.now();
```

---

# 14. ZonedDateTime

`ZonedDateTime` represents:

```text
Date
+
Time
+
Time Zone
```

Example:

```java
ZonedDateTime now =
    ZonedDateTime.now();

System.out.println(now);
```

Possible output:

```text
2026-09-28T14:30:00+05:30[Asia/Kolkata]
```

---

## Specific Zone

```java
ZonedDateTime indiaTime =
    ZonedDateTime.now(
        ZoneId.of("Asia/Kolkata")
    );
```

Another zone:

```java
ZonedDateTime londonTime =
    ZonedDateTime.now(
        ZoneId.of("Europe/London")
    );
```

---

## Why ZonedDateTime?

Useful when applications operate across time zones.

Examples:

```text
Flight schedules
International meetings
Global applications
Calendar systems
Booking systems
```

---

# 15. ZoneId

`ZoneId` identifies a time zone.

Example:

```java
ZoneId zone =
    ZoneId.of("Asia/Kolkata");
```

Another:

```java
ZoneId zone =
    ZoneId.of("America/New_York");
```

---

## System Default Zone

```java
ZoneId zone =
    ZoneId.systemDefault();

System.out.println(zone);
```

---

## List Available Zones

```java
Set<String> zones =
    ZoneId.getAvailableZoneIds();

System.out.println(
    zones.size()
);
```

---

# 16. OffsetDateTime

`OffsetDateTime` represents:

```text
Date
+
Time
+
UTC Offset
```

Example:

```java
OffsetDateTime now =
    OffsetDateTime.now();

System.out.println(now);
```

Possible output:

```text
2026-09-28T14:30:00+05:30
```

---

## Zone vs Offset

A timezone such as:

```text
Asia/Kolkata
```

is a `ZoneId`.

An offset such as:

```text
+05:30
```

is a fixed difference from UTC.

A `ZoneId` can have rules for changes over time, while an offset is simply the current/fixed offset represented.

---

# 17. Period

`Period` represents a date-based amount of time.

It works primarily with:

```text
Years
Months
Days
```

Example:

```java
Period period =
    Period.ofYears(2);

System.out.println(period);
```

---

## Years, Months, Days

```java
Period period =
    Period.of(
        2,
        3,
        10
    );
```

This means:

```text
2 years
3 months
10 days
```

---

## Between Two Dates

```java
LocalDate start =
    LocalDate.of(2024, 1, 1);

LocalDate end =
    LocalDate.of(2026, 4, 11);

Period period =
    Period.between(
        start,
        end
    );

System.out.println(
    period.getYears()
);
```

---

## Adding Period

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

Period period =
    Period.ofMonths(2);

LocalDate result =
    date.plus(period);
```

---

# 18. Duration

`Duration` represents a time-based amount.

It works with units such as:

```text
Seconds
Nanoseconds
Minutes
Hours
```

Example:

```java
Duration duration =
    Duration.ofHours(5);

System.out.println(duration);
```

---

## Minutes

```java
Duration duration =
    Duration.ofMinutes(90);
```

---

## Between Times

```java
LocalTime start =
    LocalTime.of(10, 0);

LocalTime end =
    LocalTime.of(12, 30);

Duration duration =
    Duration.between(
        start,
        end
    );

System.out.println(
    duration.toMinutes()
);
```

Output:

```text
150
```

---

## Between Date-Time Values

```java
LocalDateTime start =
    LocalDateTime.of(
        2026,
        9,
        28,
        10,
        0
    );

LocalDateTime end =
    LocalDateTime.of(
        2026,
        9,
        28,
        12,
        30
    );

Duration duration =
    Duration.between(
        start,
        end
    );
```

---

# 19. Period vs Duration

This is a common interview question.

| Period | Duration |
|---|---|
| Date-based | Time-based |
| Years | Seconds |
| Months | Minutes |
| Days | Hours |
| Works with date concepts | Works with time concepts |
| `LocalDate` commonly used | `LocalTime` / `LocalDateTime` / `Instant` commonly used |

Mental trick:

```text
Period
→ calendar-based

Duration
→ clock-based
```

Example:

```text
2 years 3 months 5 days
→ Period

48 hours
→ Duration
```

---

# 20. Comparing Dates and Times

Date/time classes provide:

```text
isBefore()
isAfter()
isEqual()
```

Example:

```java
LocalDate first =
    LocalDate.of(2026, 9, 20);

LocalDate second =
    LocalDate.of(2026, 9, 28);

System.out.println(
    first.isBefore(second)
);
```

Output:

```text
true
```

---

## isAfter()

```java
System.out.println(
    second.isAfter(first)
);
```

---

## isEqual()

```java
LocalDate date1 =
    LocalDate.of(2026, 9, 28);

LocalDate date2 =
    LocalDate.of(2026, 9, 28);

System.out.println(
    date1.isEqual(date2)
);
```

Output:

```text
true
```

---

# 21. Formatting Dates

Formatting means:

```text
Date/Time object
        ↓
String
```

Use:

```text
DateTimeFormatter
```

Example:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy"
    );

String result =
    date.format(formatter);

System.out.println(result);
```

Output:

```text
28-09-2026
```

---

# 22. Parsing Dates

Parsing means:

```text
String
   ↓
Date/Time object
```

Example:

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy"
    );

LocalDate date =
    LocalDate.parse(
        "28-09-2026",
        formatter
    );

System.out.println(date);
```

---

## Formatting vs Parsing

```text
Formatting
Object → String

Parsing
String → Object
```

Remember:

```text
format
→ output String

parse
→ input String
```

---

# 23. DateTimeFormatter

`DateTimeFormatter` is used for formatting and parsing.

Example:

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd/MM/yyyy HH:mm:ss"
    );
```

Example:

```java
LocalDateTime now =
    LocalDateTime.now();

String result =
    now.format(formatter);

System.out.println(result);
```

---

## Common Pattern Symbols

| Symbol | Meaning |
|---|---|
| `yyyy` | Year |
| `MM` | Month |
| `dd` | Day |
| `HH` | Hour, 24-hour |
| `mm` | Minute |
| `ss` | Second |
| `SSS` | Millisecond |
| `E` | Day name |
| `a` | AM/PM |

Example:

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy HH:mm:ss"
    );
```

---

# 24. ChronoUnit

`ChronoUnit` provides standard units for date/time calculations.

Examples:

```text
DAYS
WEEKS
MONTHS
YEARS
HOURS
MINUTES
SECONDS
```

Example:

```java
LocalDate start =
    LocalDate.of(2026, 9, 1);

LocalDate end =
    LocalDate.of(2026, 9, 28);

long days =
    ChronoUnit.DAYS.between(
        start,
        end
    );

System.out.println(days);
```

Output:

```text
27
```

---

## Hours

```java
LocalDateTime start =
    LocalDateTime.of(
        2026,
        9,
        28,
        10,
        0
    );

LocalDateTime end =
    LocalDateTime.of(
        2026,
        9,
        28,
        18,
        0
    );

long hours =
    ChronoUnit.HOURS.between(
        start,
        end
    );
```

---

# 25. ChronoField

`ChronoField` represents individual fields of date/time.

Examples:

```text
YEAR
MONTH_OF_YEAR
DAY_OF_MONTH
DAY_OF_YEAR
HOUR_OF_DAY
MINUTE_OF_HOUR
SECOND_OF_MINUTE
```

Example:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

int year =
    date.get(
        ChronoField.YEAR
    );
```

---

## Example

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

int day =
    date.get(
        ChronoField.DAY_OF_MONTH
    );
```

---

# 26. Leap Year

`LocalDate` provides:

```text
isLeapYear()
```

Example:

```java
LocalDate date =
    LocalDate.of(2024, 1, 1);

System.out.println(
    date.isLeapYear()
);
```

Output:

```text
true
```

For 2026:

```java
LocalDate date =
    LocalDate.of(2026, 1, 1);

System.out.println(
    date.isLeapYear()
);
```

Output:

```text
false
```

---

# 27. Date Arithmetic

Java's Date/Time API provides readable arithmetic methods.

Example:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);
```

Add days:

```java
LocalDate result =
    date.plusDays(10);
```

Subtract days:

```java
LocalDate result =
    date.minusDays(10);
```

Add months:

```java
LocalDate result =
    date.plusMonths(2);
```

Subtract years:

```java
LocalDate result =
    date.minusYears(1);
```

---

## with()

You can replace individual components.

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

LocalDate result =
    date.withDayOfMonth(15);
```

---

# 28. Immutability

The Java 8 date/time classes are generally immutable.

This is extremely important.

Consider:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

date.plusDays(10);

System.out.println(date);
```

The original date does not change.

You must store the returned object:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);

date =
    date.plusDays(10);
```

Now `date` refers to the new value.

---

## Mental Model

```text
LocalDate
    |
    | plusDays()
    ↓
New LocalDate
```

Not:

```text
LocalDate
    |
    | plusDays()
    ↓
Same object modified
```

---

# 29. Thread Safety

The modern `java.time` classes are immutable and thread-safe.

This makes them much easier to use safely across threads.

For example:

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy"
    );
```

`DateTimeFormatter` is immutable and thread-safe.

This is an important difference from:

```text
SimpleDateFormat
```

which is not thread-safe.

---

# 30. Legacy Date Conversion

Modern applications may still interact with old APIs.

Java provides conversion methods.

---

## Date to Instant

```java
Date oldDate =
    new Date();

Instant instant =
    oldDate.toInstant();
```

---

## Instant to Date

```java
Instant instant =
    Instant.now();

Date oldDate =
    Date.from(instant);
```

---

## Important

`Date` and `Instant` do not mean exactly the same thing in API design, but conversion is straightforward when integrating legacy code.

---

# 31. Common Mistakes

## ❌ Mistake 1 — Using LocalDate for a Timestamp

This:

```java
LocalDate date =
    LocalDate.now();
```

contains no time.

For date + time:

```java
LocalDateTime
```

For a global moment:

```java
Instant
```

---

## ❌ Mistake 2 — Thinking LocalDateTime Has a Timezone

It does not.

```text
LocalDateTime
→ date + time

ZonedDateTime
→ date + time + timezone
```

---

## ❌ Mistake 3 — Expecting plusDays() to Modify the Object

Wrong mental model:

```java
date.plusDays(5);
```

and expecting `date` to change.

Correct:

```java
date =
    date.plusDays(5);
```

---

## ❌ Mistake 4 — Confusing Month Values

With `LocalDate.of()`:

```java
LocalDate date =
    LocalDate.of(2026, 9, 28);
```

September is:

```text
9
```

Unlike some old APIs, you do not use:

```text
8 for September
```

---

## ❌ Mistake 5 — Confusing `MM` and `mm`

In format patterns:

```text
MM
→ month

mm
→ minute
```

For example:

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy HH:mm"
    );
```

---

## ❌ Mistake 6 — Using `Duration` for Calendar Months

A month is not a fixed number of hours.

For calendar-based amounts, use:

```text
Period
```

---

## ❌ Mistake 7 — Using System Default Time Zone in Global Systems Without Thinking

Code such as:

```java
ZonedDateTime.now();
```

uses the system's default timezone.

For predictable global behavior, explicitly specify the required zone when appropriate.

---

# 32. Interview Questions

## 🔥 Q1. Why was the Java 8 Date/Time API introduced?

To provide a cleaner, immutable, thread-safe and more expressive API for date/time operations than older APIs such as `Date`, `Calendar`, and `SimpleDateFormat`.

---

## 🔥 Q2. What package contains the Java 8 Date/Time API?

Primarily:

```text
java.time
```

---

## 🔥 Q3. What is LocalDate?

`LocalDate` represents a date without time or timezone.

```text
Year + Month + Day
```

---

## 🔥 Q4. What is LocalTime?

`LocalTime` represents a time without date or timezone.

```text
Hour + Minute + Second + Nano
```

---

## 🔥 Q5. What is LocalDateTime?

It combines:

```text
LocalDate
+
LocalTime
```

but contains no timezone or offset.

---

## 🔥 Q6. Does LocalDateTime represent an exact global moment?

Not by itself.

Without timezone/offset information, the same local date-time can occur in multiple locations.

---

## 🔥 Q7. What is Instant?

`Instant` represents a point on the UTC timeline.

It is useful for timestamps and globally comparable moments.

---

## 🔥 Q8. What is ZonedDateTime?

It represents:

```text
Date + Time + Time Zone
```

---

## 🔥 Q9. Difference between ZoneId and ZoneOffset?

```text
ZoneId
→ identifies a region/time-zone rule set

ZoneOffset
→ fixed offset from UTC
```

Examples:

```text
Asia/Kolkata
Europe/London
```

versus:

```text
+05:30
+00:00
```

---

## 🔥 Q10. Difference between LocalDateTime and ZonedDateTime?

```text
LocalDateTime
→ date + time

ZonedDateTime
→ date + time + timezone
```

---

## 🔥 Q11. What is Period?

`Period` represents date-based amounts such as:

```text
Years
Months
Days
```

---

## 🔥 Q12. What is Duration?

`Duration` represents time-based amounts such as:

```text
Seconds
Minutes
Hours
```

---

## 🔥 Q13. Difference between Period and Duration?

```text
Period
→ calendar/date based

Duration
→ clock/time based
```

---

## 🔥 Q14. Are Java 8 Date/Time classes immutable?

Yes, the core modern date/time value classes are immutable.

Operations such as `plusDays()` return new objects.

---

## 🔥 Q15. Why is immutability useful here?

It makes date/time values safer to share and easier to reason about, and contributes to thread safety.

---

## 🔥 Q16. Is DateTimeFormatter thread-safe?

Yes.

`DateTimeFormatter` is immutable and thread-safe.

---

## 🔥 Q17. Is SimpleDateFormat thread-safe?

No.

This is one reason the newer API is preferred.

---

## 🔥 Q18. How do you format a LocalDate?

Use `DateTimeFormatter`.

```java
DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
        "dd-MM-yyyy"
    );

String result =
    date.format(formatter);
```

---

## 🔥 Q19. Difference between formatting and parsing?

```text
Formatting
Date/Time → String

Parsing
String → Date/Time
```

---

## 🔥 Q20. How do you calculate the number of days between two dates?

Using `ChronoUnit`:

```java
long days =
    ChronoUnit.DAYS.between(
        start,
        end
    );
```

---

## 🔥 Q21. What is ChronoUnit?

It provides standard units such as:

```text
DAYS
WEEKS
MONTHS
YEARS
HOURS
MINUTES
SECONDS
```

for date/time calculations.

---

## 🔥 Q22. What is ChronoField?

It represents individual date/time fields such as:

```text
YEAR
MONTH_OF_YEAR
DAY_OF_MONTH
HOUR_OF_DAY
```

---

## 🔥 Q23. How do you check whether a year is leap year?

Using `isLeapYear()`:

```java
boolean leap =
    date.isLeapYear();
```

---

## 🔥 Q24. Why is LocalDate better than old Date for date-only values?

Because it clearly represents a date without unnecessary time or timezone semantics and has an immutable, cleaner API.

---

## 🔥 Q25. When should you use Instant?

Use `Instant` when you need a globally comparable point in time, such as:

```text
timestamps
logs
events
database/server timestamps
```

---

## 🔥 Q26. When should you use ZonedDateTime?

Use it when the timezone or region's time-zone rules are part of the meaning of the value.

Examples:

```text
international meetings
flight schedules
global calendars
```

---

## 🔥 Q27. Why shouldn't you use LocalDateTime for every timestamp?

Because it does not contain timezone or offset information and therefore does not uniquely identify a global moment.

---

## 🔥 Q28. What happens when `plusDays()` is called?

A new date object is returned because the original date object is immutable.

---

## 🔥 Q29. What is the difference between `MM` and `mm`?

```text
MM
→ month

mm
→ minute
```

---

## 🔥 Q30. What is the biggest advantage of the Java 8 Date/Time API?

Its API clearly separates different concepts:

```text
Date
Time
Date + Time
Instant
Timezone
Duration
Period
Formatting
```

This makes code easier to understand, safer, and less error-prone.

---

# 33. 30-Second Interview Answer

> Java 8 introduced the `java.time` API to replace many problematic use cases of `Date`, `Calendar`, and `SimpleDateFormat`. Important classes include `LocalDate`, `LocalTime`, `LocalDateTime`, `Instant`, `ZonedDateTime`, `Period`, and `Duration`. The API uses immutable and thread-safe value types. `LocalDate` represents a date, `LocalTime` represents a time, `LocalDateTime` represents date plus time without a zone, `Instant` represents a point on the UTC timeline, and `ZonedDateTime` includes timezone information. `Period` is mainly calendar-based, while `Duration` is time-based.

---

# 34. Cheat Sheet

```text
================ JAVA TIME CHEAT SHEET ================


PACKAGE
---------------------------------

java.time


LOCALDATE
---------------------------------

Date only

Year
Month
Day

Example:

LocalDate.now()


LOCALTIME
---------------------------------

Time only

Hour
Minute
Second
Nano


LOCALDATETIME
---------------------------------

Date + Time

NO timezone
NO offset


INSTANT
---------------------------------

Point on UTC timeline

Useful for:
timestamps
logs
events


ZONEDDATETIME
---------------------------------

Date + Time + Zone


ZONEID
---------------------------------

Region-based timezone

Example:

Asia/Kolkata


OFFSETDATETIME
---------------------------------

Date + Time + UTC Offset

Example:

+05:30


PERIOD
---------------------------------

Date-based amount

Years
Months
Days


DURATION
---------------------------------

Time-based amount

Hours
Minutes
Seconds
Nanos


PERIOD VS DURATION
---------------------------------

Period
→ calendar

Duration
→ clock


FORMAT
---------------------------------

Object → String

date.format(formatter)


PARSE
---------------------------------

String → Object

LocalDate.parse(
    text,
    formatter
)


FORMATTER
---------------------------------

DateTimeFormatter


COMPARISON
---------------------------------

isBefore()
isAfter()
isEqual()


DATE ARITHMETIC
---------------------------------

plusDays()
minusDays()

plusMonths()
minusMonths()

plusYears()
minusYears()


IMMUTABILITY
---------------------------------

plus/minus/with
→ return NEW object


CHRONOUNIT
---------------------------------

DAYS
WEEKS
MONTHS
YEARS
HOURS
MINUTES
SECONDS


CHRONOFIELD
---------------------------------

YEAR
MONTH_OF_YEAR
DAY_OF_MONTH
DAY_OF_YEAR
HOUR_OF_DAY
...


LEAP YEAR
---------------------------------

date.isLeapYear()


LEGACY CONVERSION
---------------------------------

Date → Instant

date.toInstant()


Instant → Date

Date.from(instant)


IMPORTANT FORMAT TRAP
---------------------------------

MM → month

mm → minute


OLD API
---------------------------------

Date
Calendar
SimpleDateFormat


MODERN API
---------------------------------

LocalDate
LocalTime
LocalDateTime
Instant
ZonedDateTime
DateTimeFormatter


IMPORTANT
---------------------------------

LocalDateTime
≠
global timestamp


====================================================
```

---

# 35. Final Mental Model

```text
                         JAVA TIME API
                              |
              +---------------+---------------+
              |               |               |
             DATE            TIME          DATE + TIME
              |               |               |
         LocalDate        LocalTime      LocalDateTime
              |               |               |
              +---------------+---------------+
                              |
                       NO TIMEZONE


                              |
                              ↓
                         TIME ZONE
                              |
                     +--------+--------+
                     |                 |
                   ZoneId        ZonedDateTime
                     |                 |
              Asia/Kolkata       Date + Time
              Europe/London           +
                                Timezone


                              |
                              ↓
                           INSTANT
                              |
                         UTC TIMELINE
                              |
                       Global moment


====================================================

AMOUNT OF TIME

             +-------------------------+
             |                         |
          Period                    Duration
             |                         |
       Years/Months/Days          Hours/Minutes
       Calendar-based             Seconds/Nanos
             |                         |
         LocalDate              Time/DateTime
                                  / Instant


====================================================

STRING CONVERSION

Date/Time Object
       |
       | format()
       ↓
     String


String
       |
       | parse()
       ↓
Date/Time Object


====================================================

DATE COMPARISON

Date A
  |
  +── isBefore(Date B)
  |
  +── isAfter(Date B)
  |
  +── isEqual(Date B)


====================================================

DATE MODIFICATION

LocalDate
    |
    +── plusDays()
    |
    +── minusDays()
    |
    +── plusMonths()
    |
    +── minusMonths()
    |
    +── withYear()
    |
    ↓
NEW LocalDate


====================================================

MOST IMPORTANT DIFFERENCES

LocalDate
→ date only

LocalTime
→ time only

LocalDateTime
→ date + time

Instant
→ global UTC moment

ZonedDateTime
→ date + time + timezone

Period
→ calendar amount

Duration
→ time amount


====================================================

INTERVIEW MEMORY TRICK

D → Date
T → Time
DT → Date + Time
I → Instant
Z → Zone
P → Period
D → Duration

LocalDate
LocalTime
LocalDateTime
Instant
ZonedDateTime
Period
Duration


====================================================

JAVA 8 DATE/TIME CORE IDEA

Don't think:

"Which old Date method should I use?"

Think:

"What does my value actually represent?"

Date only
     ↓
LocalDate

Time only
     ↓
LocalTime

Date + Time
     ↓
LocalDateTime

Global moment
     ↓
Instant

Date + Time + Zone
     ↓
ZonedDateTime

Calendar amount
     ↓
Period

Clock amount
     ↓
Duration
```

---

# 🏁 Final Takeaways

- Java 8 introduced the modern `java.time` API.
- It was designed to address limitations of older date/time APIs.
- `LocalDate` represents a date without time or timezone.
- `LocalTime` represents a time without date or timezone.
- `LocalDateTime` represents date + time without timezone.
- `Instant` represents a point on the UTC timeline.
- `ZonedDateTime` represents date + time + timezone.
- `ZoneId` identifies a region-based timezone.
- `OffsetDateTime` represents date + time + UTC offset.
- `Period` is primarily calendar/date based.
- `Duration` is primarily time based.
- Java time classes are generally immutable.
- Date/time modification returns new objects.
- `DateTimeFormatter` is used for formatting and parsing.
- Formatting converts an object into a String.
- Parsing converts a String into a date/time object.
- `ChronoUnit` provides standard time units for calculations.
- `ChronoField` represents individual date/time fields.
- `isBefore()`, `isAfter()`, and `isEqual()` are used for comparison.
- `isLeapYear()` checks leap years.
- `MM` means month while `mm` means minute.
- `LocalDateTime` should not be treated as a global timestamp because it has no timezone or offset.
- `Instant` is appropriate when the exact global point in time matters.
- `ZonedDateTime` is appropriate when timezone rules are part of the meaning.
- `DateTimeFormatter` is immutable and thread-safe.
- `SimpleDateFormat` is not thread-safe.
- The biggest improvement is the clear separation of date, time, timezone, duration, and period concepts.

---

