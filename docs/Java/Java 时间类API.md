
```java
package com.abc.queryparser.timeanalysis;

import java.time.*;
import java.time.chrono.Era;
import java.time.temporal.ChronoUnit;
import java.time.temporal.TemporalAdjusters;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.Date;
import java.util.Calendar;
import java.util.Locale;
import java.util.Set;


/**
 * 该类主要用来学习熟悉 Java Time API
 */
public class MyTime {

    public static void main(String args[]) {
        MyTime myTime = new MyTime();
        myTime.testLocalDate();
        myTime.testLocalTime();
        myTime.testLocalDateTime();
        myTime.testZonedDateTime();
        myTime.testChromoUnits();
        myTime.testPeriod();
        myTime.testDuration();
        myTime.testTemporalAdjusters();
        myTime.testBackwardCompatability();
        myTime.testDateTimeFormatter();
    }

    /**
     * The LocalDate represents a date in ISO format (yyyy-MM-dd) without time.
     */
    public void testLocalDate() {
        // LocalDate instance can be created from the system clock, or can be obtained using
        // the “of” method or by using the “parse” method
        LocalDate localDate1 = LocalDate.now();
        LocalDate localDate2 = LocalDate.of(2016, 2, 29); //2016-02-29
        LocalDate localDate3 = LocalDate.ofEpochDay(234); // 1970-08-23
        LocalDate localDate4 = LocalDate.ofYearDay(2015, 23); // 2015-01-23
        LocalDate localDate5 = LocalDate.parse("2015-03-30"); // 2015-03-30

        // The "add" and "minus" API.
        LocalDate nextYear =  LocalDate.parse("2016-02-29").plusYears(1); // 2017-02-28
        LocalDate beforeMonth = LocalDate.parse("2015-03-30").minusMonths(1); // 2016-02-28
        LocalDate nextWeek = LocalDate.parse("2015-03-30").plusWeeks(1); // 2016-04-06
        LocalDate beforeDay = LocalDate.parse("2015-03-30").minusDays(31); // 2015-02-27
        LocalDate previousMonthSameDay = LocalDate.parse("2015-03-30").minus(2, ChronoUnit.MONTHS); // 2016-01-30
        LocalDate previousDaySameDay =  LocalDate.parse("2015-03-30").plus(1, ChronoUnit.WEEKS); // 2016-04-06

        // Getters
        DayOfWeek dayOfWeek = LocalDate.parse("2016-06-12").getDayOfWeek(); // "SUNDAY"
        int dayOfMonth = LocalDate.parse("2016-06-12").getDayOfMonth(); // 12
        int dayOfYear = LocalDate.parse("2016-06-12").getDayOfYear(); // 164
        int year = LocalDate.parse("2016-06-12").getYear(); //2016
        Month month = LocalDate.parse("2016-06-12").getMonth(); // "JUNE"
        int monthValue = LocalDate.parse("2016-06-12").getMonthValue();  // 6
        Era era = LocalDate.parse("2016-06-12").getEra(); // "CE"

        // boolean method
        boolean leapYear = LocalDate.parse("2016-06-12").isLeapYear();  // true
        boolean isBefore = LocalDate.parse("2016-06-12").isBefore(LocalDate.parse("2016-06-11")); // false
        boolean isAfter = LocalDate.parse("2016-06-12").isAfter(LocalDate.parse("2016-06-11")); // true

        // "with" method to return a new LocalDate instance with specified value, unexistable Value will cause Exception
        LocalDate withYear = LocalDate.parse("2016-03-29").withYear(2017).withMonth(2); // 2017-02-28
        LocalDate withDayOfYear = LocalDate.parse("2016-06-12").withDayOfYear(12); // 2017-01-12
        LocalDate withDayOfMonth = LocalDate.parse("2016-06-12").withDayOfMonth(2); // 2017-06-02
        LocalDate firstDayOfMonth = LocalDate.parse("2016-06-12").with(TemporalAdjusters.firstDayOfMonth()); // 2017-06-01
        LocalDate nextOrSameSaturday = LocalDate.parse("2016-06-12").with(TemporalAdjusters.nextOrSame(DayOfWeek.SATURDAY)); // 2017-06-18
        LocalDate dayOfWeekInMonth = LocalDate.parse("2016-06-12").with(TemporalAdjusters.dayOfWeekInMonth(5, DayOfWeek.SATURDAY)); // 2017-07-02

        // "at" method used to append time to get LocalDateTime/ZonedDateTime
        LocalDateTime atTime = LocalDate.parse("2016-06-12").atTime(23, 12, 34, 2343); // 2016-06-12T23:12:34.000002343
        LocalDateTime atStartOfDay = LocalDate.parse("2016-06-12").atStartOfDay(); // 2016-06-12T00:00
        ZonedDateTime atStartOfDayZoned = LocalDate.parse("2016-06-12").atStartOfDay(ZoneId.systemDefault()); // 2016-06-12T00:00+08:00[Asia/Shanghai]

        // other methods
        long epochDay = LocalDate.parse("2016-06-12").toEpochDay(); // 16964
    }

    /**
     * The LocalTime represents time in ISO format (hh:mm:ss)  without a date.
     */
    public void testLocalTime() {
        LocalTime LocalTime1 = LocalTime.now();
        LocalTime LocalTime2 = LocalTime.parse("06:30"); // 06:30
        LocalTime LocalTime3 = LocalTime.of(6, 30, 23, 2345); // 06:30:23.000002345
        LocalTime LocalTime4 = LocalTime.ofSecondOfDay(3452); // 00:57:32

        LocalTime nextHour = LocalTime.parse("06:30").plus(1, ChronoUnit.HOURS); // 07:30
        LocalTime beforeHalfHour = LocalTime.parse("06:30").minusSeconds(30); // 06:29:30

        int hour = LocalTime.parse("06:30").getHour(); // 6

        boolean isAfter = LocalTime.parse("06:30").isAfter(LocalTime.parse("06:40")); // false

        LocalTime minute = LocalTime.parse("06:30").withMinute(2); // 06:02

        LocalDateTime atDate = LocalTime.parse("06:30").atDate(LocalDate.parse("2016-06-12")); // 2016-06-12T06:30
        OffsetTime atDateZoned = LocalTime.parse("06:30").atOffset(ZoneOffset.UTC); // 06:30Z

        long secondOfDay = LocalTime.parse("06:30").toSecondOfDay(); // 23400
        LocalTime maxTime = LocalTime.MAX; // 23:59:59.999999999
        LocalTime midnight = LocalTime.MIDNIGHT; // 00:00
    }

    /**
     * The LocalDateTime is used to represent a combination of date and time.
     */
    public void testLocalDateTime() {
        LocalDateTime localDateTime1 = LocalDateTime.now();
        LocalDateTime localDateTime2 = LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30); // 2015-02-20T06:30
        LocalDateTime localDateTim3 = LocalDateTime.parse("2015-02-20T06:30:00"); // 2015-02-20T06:30
        LocalDateTime localDateTime4 = LocalDateTime.ofEpochSecond(1465817690, 0, ZoneOffset.UTC); // 2016-06-13T11:34:50

        LocalDateTime nextMonth = LocalDateTime.parse("2015-01-30T06:30:00").plusMonths(1); // 2015-02-28T06:30
        LocalDateTime beforeWeek = LocalDateTime.parse("2015-01-30T06:30:00").minus(7, ChronoUnit.WEEKS); // 2015-12-12T06:30:00

        int dayofMonth = LocalDateTime.parse("2015-02-20T06:30:00").getDayOfMonth(); // 20

        boolean isEqual = LocalDateTime.parse("2015-02-20T06:30:00").isEqual(LocalDateTime.parse("2015-02-20T06:30")); // true

        LocalDateTime withDayOfMonth = LocalDateTime.parse("2015-02-20T06:30:00").withDayOfMonth(23); // 2015-02-23T06:30

        ZonedDateTime atZone = LocalDateTime.parse("2015-02-20T06:30:00").atZone(ZoneOffset.UTC); // 2015-02-20T06:30Z
        LocalDate toLocalDate = LocalDateTime.parse("2015-02-20T06:30:00").toLocalDate(); // 2015-02-20
        LocalTime toLocalTime = LocalDateTime.parse("2015-02-20T06:30:00").toLocalTime(); // 06:30
        LocalDateTime max =LocalDateTime.MAX; // +999999999-12-31T23:59:59.999999999
    }

    /**
     * Zoned date-time API is to be used when time zone is to be considered.
     */
    public void testZonedDateTime() {
        ZoneId currentZoneId = ZoneId.systemDefault(); // Asia/Shanghai
        ZoneId zoneId = ZoneId.of("Europe/Paris"); // Europe/Paris
        Set<String> allZoneIds = ZoneId.getAvailableZoneIds(); // 599 ZoneIds

        ZonedDateTime zonedDateTime1 = ZonedDateTime.parse("2007-07-31T10:15:30+05:30[Asia/Karachi]"); // 2007-07-31T10:15:30+05:00[Asia/Karachi]
        LocalDateTime localDateTime = LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30); // 2015-02-20T06:30
        ZonedDateTime zonedDateTime2 = ZonedDateTime.of(localDateTime, zoneId); // 2015-02-20T06:30+01:00[Europe/Paris]

        ZoneOffset offset = ZoneOffset.of("+02:00"); // +02:00
        OffsetDateTime offSetByTwo = OffsetDateTime.of(localDateTime, offset);  // 2015-02-20T06:30+02:00

        int dayOfMonth = zonedDateTime1.getDayOfMonth(); // 31
        ZonedDateTime nextMonth = zonedDateTime1.plus(1, ChronoUnit.MONTHS); // 2007-08-31T10:15:30+05:00[Asia/Karachi]
        ZonedDateTime withDayOfMonth = zonedDateTime1.withDayOfMonth(22); // 2007-07-22T10:15:30+05:00[Asia/Karachi]
        LocalDateTime toLocalDateTime = zonedDateTime1.toLocalDateTime(); // 2007-07-31T10:15:30
        OffsetDateTime toOffsetDateTime = zonedDateTime1.toOffsetDateTime(); // 2007-07-31T10:15:30+05:00
        LocalDate toLocalDate = zonedDateTime1.toLocalDate(); // 2007-07-31
        LocalTime toLocalTime = zonedDateTime1.toLocalTime(); // 10:15:30
    }

    /**
     * ChronoUnit enum is added in Java 8 to replace the integer values used in old API to represent day, month, etc.
     */
    public void testChromoUnits() {
        LocalDate today = LocalDate.now(); // 2018-06-14
        LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS); // 2018-06-21
        LocalDate nextMonth = today.plus(1, ChronoUnit.MONTHS); // 2018-07-14
        LocalDate nextYear = today.plus(1, ChronoUnit.YEARS); // 2019-06-14
        LocalDate nextDecade = today.plus(1, ChronoUnit.DECADES); // 2028-06-14
    }

    /**
     * The Period class is widely used to modify values of given a date or to obtain the difference between two dates:
     */
    public void testPeriod() {
        Period period1 = Period.of(8,2,11); // P8Y2M11D
        Period period2 = Period.parse("P1Y2M3W4D"); // P1Y2M25D
        LocalDate date1 = LocalDate.parse("2018-03-31");  // 2018-03-31
        LocalDate date2 = date1.minus(1, ChronoUnit.MONTHS); // 2018-02-28
        Period period3 = Period.between(date1, date2);  // P-1M-3D
        Period period4 = period3.withYears(1);  // P1Y-1M-3D
        int days = period3.getDays(); // -3
    }

    /**
     * Similar to Period, the Duration class is use to deal with Time.
     */
    public void testDuration() {
        Duration duration1 = Duration.ofHours(2); // PT2H
        Duration duration2 = Duration.parse("P-2DT3H4M"); // PT-44H-56M
        LocalTime time1 = LocalTime.parse("13:34:23"); // 13:34:23
        LocalTime time2 = time1.plus(Duration.ofSeconds(30)); // 13:34:53
        Duration duration3 = Duration.between(time1, time2); // PT30S
        Duration duration4 = duration3.withSeconds(23); // PT23S
        long seconds = duration4.getSeconds(); // 23
    }

    /**
     * TemporalAdjuster is used to perform the date mathematics.
     * For example, get the "Second Saturday of the Month" or "Next Tuesday".
     */
    public void testTemporalAdjusters() {
        LocalDate date1 = LocalDate.now(); // 2018-06-14
        LocalDate nextTuesday = date1.with(TemporalAdjusters.next(DayOfWeek.TUESDAY)); // 2018-06-19
        LocalDate firstInYear = LocalDate.of(date1.getYear(),date1.getMonth(), 1); // 2018-06-01
        LocalDate secondSaturday = firstInYear.with(TemporalAdjusters.nextOrSame(
                DayOfWeek.SATURDAY)).with(TemporalAdjusters.next(DayOfWeek.SATURDAY)); // 2018-06-09
    }

    /**
     * The toInstant() method which helps to convert existing Date and Calendar instance to new Date Time API
     */
    public void testBackwardCompatability() {
        Date date = new Date(); // Thu Jun 14 20:59:43 CST 2018
        Calendar calendar = Calendar.getInstance();
        LocalDateTime localDateTime = LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault()); // 2018-06-14T20:59:43.368
        ZonedDateTime zonedDateTime = ZonedDateTime.ofInstant(calendar.toInstant(), ZoneId.systemDefault()); // 2018-06-14T20:59:43.374+08:00[Asia/Shanghai]

        Date date1 = Date.from(localDateTime.toInstant(ZoneOffset.UTC)); // Fri Jun 15 04:59:43 CST 2018
    }

    /**
     *
     */
    public void testDateTimeFormatter() {
        LocalDateTime localDateTime = LocalDateTime.parse("2015-01-25T06:30"); // 2015-01-25T06:30
        String localDateString1 = localDateTime.format(DateTimeFormatter.ISO_DATE); // 2015-01-25
        String localDateString2 = localDateTime.format(DateTimeFormatter.ofPattern("yyyy/MM/dd hh:mm:ss")); // 2015/01/25 06:30:00
        String localDateString3 = localDateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM).withLocale(Locale.UK)); // 25-Jan-2015 06:30:00
    }
}
```
