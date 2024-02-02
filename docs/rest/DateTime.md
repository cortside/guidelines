# Date and Time handling

While we all deal with dates and times on a regular basis, they require some special considerations in relation to apis.

## Law #1: Use ISO-8601 for your dates

[ISO-8601](https://www.iso.org/iso-8601-date-and-time-format.html) has been accepted ubiquitously as the default standard for handing of dates and times and all of their related combinations.  Diverting from anything other than ISO-8601 will cause confusion and problems consumers of your api.

Examples:

```json
{
  // ...
  "createdDate": "2022-01-16T17:52:52.848Z",
  "lastModifiedDate": "2022-01-16T17:52:52.848Z",
  "timestamp": "2024-02-02T11:10:35.2992216-07:00",
  "uptime": "P0Y0M1DT9H1M46S",
  "birthdate": "2000-01-07",
  // ...
}
```

The consumer is responsible for doing any time zone conversion that may be needed.

## Law #2: Accept any timezone

Given that consumers of your api may be in any timezone and their timezone should not be assumed.  As long as date times are communicated with UTC offset, they can be adjusted to UTC by your service

## Law #3: Store it in UTC

Hosting servers and databases ideally all operate with their timezone set as UTC, regardless of location.  This is almost always, if not always true for cloud services like AWS, Azure or Google.  Data persistence in UTC is especially important persistence systems that don't support a data type that can fully represent a date and time with timezone or when a type is chosen that does not support it (i.e. MS SQL datetime or datetime2).

## Law #4: Return it in UTC

Unlike accepting any timezone, an api should always return date times in UTC.  This is helpful consumers that use languages that can't keep the timezone.

The consumer is always responsible for converting UTC to desirable timezone for use, i.e. using user's local timezone from their browser.


## Other

* Use a date only when only the date matters.  This is specifically true of a birthdate but could be true of a project completion date as well.
* User a time only when only the time matters.  This is specifically important for systems that have recurring events that need to happen at the same time of the day, regardless of influences like daylight savings.
