# [Optional] ZonedDateTime Class

## Learning Goals

- Learn about the `ZonedDateTime` class.

## Introduction

So far, in the new Date and Time API, we have seen the `LocalDate`, `LocalTime`,
and `LocalDateTime` classes. The one thing each of those classes don't contain
is a timezone. This is where we can implement the `ZonedDateTime` class!

## Java's `ZonedDateTime` Class

The `ZonedDateTime` class lives in the `java.time` package with the other
date-time classes that were introduces in the Java 8 Date Time API. It is an
immutable representation of a date-time with a timezone and stores all date and
time fields to a precision of nanoseconds. If you are writing code where all
date-time fields matter, including the timezone, then this is the preferred
class that you should implement.

### Constructing a `ZonedDateTime`

Creating a `ZonedDateTime` instance is similar to creating a `LocalDateTime`
object, except we specify the timezone as well. Consider the following:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.LocalTime;
import java.time.LocalDate;
import java.time.LocalDateTime;

public class ZonedDateTimeExample {

    public static void main(String[] args) {
        
        // Obtains the current date-time
        ZonedDateTime zonedDateTime = ZonedDateTime.now();
        System.out.println(zonedDateTime);

        // Specify each field of a date-time object including the timezone
        zonedDateTime = ZonedDateTime.of(2015, 10, 21, 19, 28, 0, 0, ZoneId.of("America/Los_Angeles"));
        System.out.println(zonedDateTime);

        LocalDate date = LocalDate.of(1955, 11, 5);
        LocalTime time = LocalTime.of(6, 15, 0);
        
        // Create a ZonedDateTime instance using a LocalDate and LocalTime
        zonedDateTime = ZonedDateTime.of(date, time, ZoneId.of("America/Los_Angeles"));
        System.out.println(zonedDateTime);

        LocalDateTime dateTime  = LocalDateTime.of(1985, 10, 26, 1, 35, 0);
        
        // Create a ZonedDateTime instance using a LocalDateTime
        zonedDateTime = ZonedDateTime.of(dateTime, ZoneId.of("America/Los_Angeles"));
        System.out.println(zonedDateTime);
    }
}
```

The code above may print the following results (note - the current time here
versus the current time you may obtain will be different based on the time
the program was executed).

```text
2022-09-13T21:51:10.908307-06:00[America/Denver]
2015-10-21T19:28-07:00[America/Los_Angeles]
1955-11-05T06:15-08:00[America/Los_Angeles]
1985-10-26T01:35-07:00[America/Los_Angeles]
```

As we can see in the example, the `ZoneId` class is how we can specify a certain
timezone. To get a list of all the available timezones that `ZoneId` supports,
try running the following program:

```java
import java.util.Set;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class AvailableTimeZones {

    public static void main(String[] args) {
        Set<String> zoneIds = ZoneId.getAvailableZoneIds();

        for (String zone : zoneIds) {
            System.out.println(zone);
        }
    }
}
```

If we were to run the above code, we would print out a list of 601 supported
timezones that we can use!

### Common Methods

Like the other date-time objects, the `ZonedDateTime` shares many of the same
methods. Consider the table of methods below:

| Method                               | Description                                                                                |
|--------------------------------------|--------------------------------------------------------------------------------------------|
| getDayOfMonth()                      | Gets the day of the month                                                                  |
| getHour()                            | Gets the hour of the day                                                                   |
| getMinute()                          | Gets the minute of the hour                                                                |
| getMonth()                           | Gets the enum `Month` (e.g. Month.APRIL)                                                   |
| getMonthValue()                      | Gets the integer representation of a month (e.g. 4 to represent April)                     |
| getNano()                            | Gets the nanosecond field                                                                  |
| getSecond()                          | Gets the second of the minute                                                              |
| getYear()                            | Gets the year field                                                                        |
| getZone()                            | Gets the timezone, such as 'EuropeParis'                                                   |
| minusDays(long daysToSubtract)       | Returns a copy of this `ZonedDateTime` with the specified number of days subtracted        |
| minusHours(long hoursToSubtract)     | Returns a copy of this `ZonedDateTime` with the specified number of hours subtracted       |
| minusMinutes(long minutesToSubtract) | Returns a copy of this `ZonedDateTime` with the specified number of minutes subtracted     |
| minusMonths(long monthsToSubtract)   | Returns a copy of this `ZonedDateTime` with the specified number of months subtracted      |
| minusNanos(long nanosToSubtract)     | Returns a copy of this `ZonedDateTime` with the specified number of nanoseconds subtracted |
| minusSeconds(long secondsToSubtract) | Returns a copy of this `ZonedDateTime` with the specified number of seconds subtracted     |
| minusWeeks(long weeksToSubtract)     | Returns a copy of this `ZonedDateTime` with the specified number of weeks subtracted       |
| minusYears(long yearsToSubtract)     | Returns a copy of this `ZonedDateTime` with the specified number of years subtracted       |
| plusDays(long daysToAdd)             | Returns a copy of this `ZonedDateTime` with the specified number of days added             |
| plusHours(long hoursToAdd)           | Returns a copy of this `ZonedDateTime` with the specified number of hours added            |
| plusMinutes(long minutesToAdd)       | Returns a copy of this `ZonedDateTime` with the specified number of minutes added          |
| plusMonths(long monthsToAdd)         | Returns a copy of this `ZonedDateTime` with the specified number of months added           |
| plusNanos(long nanosToAdd)           | Returns a copy of this `ZonedDateTime` with the specified number of nanoseconds added      |
| plusSeconds(long secondsToAdd)       | Returns a copy of this `ZonedDateTime` with the specified number of seconds added          |
| plusWeeks(long weeksToAdd)           | Returns a copy of this `ZonedDateTime` with the specified number of weeks added            |
| plusYears(long yearsToAdd)           | Returns a copy of this `ZonedDateTime` with the specified number of years added            |
| toLocalDate()                        | Gets the `LocalDate` part of this date-time object                                         |
| toLocalTime                          | Gets the `LocalTime` part of this date-time object                                         |
| toLocalDateTime()                    | Gets the `LocalDateTime` part of this date-time object                                     |

## Formatting a `ZonedDateTime` object

Formatting a `ZonedDateTime` object is similar to formatting the other date-time
objects. We just have the option to include a timezone now!

Let's say we want to format the `ZonedDateTime` instance to be in the format:
"MM-dd-yyyy HH:mm:ss z" where "z" represents the timezone:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;

public class ZonedDateTimeExample {

    public static void main(String[] args) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM-dd-uuuu HH:mm:ss z");

        ZonedDateTime dateTime = ZonedDateTime.of(2015, 10, 21, 19, 28, 0, 0, ZoneId.of("America/Los_Angeles"));
        String formattedDate = formatter.format(dateTime);
        System.out.println(formattedDate);
    }
}
```

The example above would produce the following result:

```text
10-21-2015 19:28:00 PDT
```

## Parsing a `ZonedDateTime` object

Just like the other date-time objects, we can also parse a `ZonedDateTime`
object using the `parse()` methods. It should be noted that the default format
for a `ZonedDateTime` instance is uuuu-MM-ddTHH:mmXV. In this format, the T
separates the date from the time portion of the date-time object and the X and
V relate to the zone-offset and zone ID respectively.

We can parse a `ZonedDateTime` object in the following manner:

```java
import java.time.ZonedDateTime;
import java.time.ZoneId;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class Example {

    public static void main(String[] args) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM-dd-uuuu HH:mm:ss z");

        // Parse the String using the default format to obtain an instance of ZonedDateTime
        ZonedDateTime dateTime = ZonedDateTime.parse("1955-11-05T06:15-08:00[America/Los_Angeles]");
        System.out.println(dateTime);
        
        // Parse the String using a specific DateTimeFormatter pattern to obtain an instance of ZonedDateTime
        dateTime = ZonedDateTime.parse("11-05-1955 06:15:00 PDT", formatter);
        System.out.println(dateTime);
        
        /* Parse the String using the default format of the LocalDateTime class and then convert it to
           a ZonedDateTime instance by using the atZone() method which combines this date-time object with
           a timezone     
         */
        dateTime = LocalDateTime.parse("1955-11-05T06:15:00").atZone(ZoneId.of("America/Los_Angeles"));
        System.out.println(dateTime);
    }
}
```

All the ways to convert a `String` to a `ZonedDateTime` in the above example will
result in the same output, no matter which way we choose to parse the `String`
object:

```text
1955-11-05T06:15-08:00[America/Los_Angeles]
1955-11-05T06:15-08:00[America/Los_Angeles]
1955-11-05T06:15-08:00[America/Los_Angeles]
```

## Resources

- [Java 17 ZonedDateTime Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/ZonedDateTime.html)
- [Java 17 ZoneId](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/ZoneId.html)

