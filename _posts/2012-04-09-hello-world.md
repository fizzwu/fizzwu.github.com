---
layout: post
title: "Hello World"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Rails Date Formats – strftime

StrFTime Format Codes for Ruby on Rails
Year
%Y     year with century 2007
%y     year without century 07
%C     century number (year divided by 100) 20

Month
%B     full month name January
%b     abbreviated month name Jan
%h     same as %b Jan
%m     month as number (01-12)

Week
%U     week number of the year, Sunday as first day of week (00-53)
%W     week number of the year, Monday as first day of week (00-53)

Day
%A     full weekday name Wednesday
%a     abbreviated weekday name Wed
%d     day of the month (01-31)
%e     day of the month, single digits preceded by space ( 1-31)
%j     day of the year (001-366)
%w     weekday as a number, with 0 representing Sunday (0-6)
%u     weekday as a number, with 1 representing Monday (1-7)

Time
%H    hour (24-hour clock) (00-23)
%k     hour (24-hour clock); single digits preceded by space ( 0-23)
%I     hour (12-hour clock) (01-12)
%l     hour (12-hour clock); single digits preceded by space ( 1-12)
%M     minute (00-59)
%S     seconds (00-59)
%p     either AM or PM AM
%Z     timezone name or abbreviation EDT
%z     timezone offset from UTC -0400

Summaries
%D     date, same as %m/%d/%y 05/16/07
%v     date, same as %e-%b-%Y 16-May-2007
%F     date, same as %Y-%m-%d 2007-05-16
%R     time, 24 hour notation, same as %H:%M 18:06
%T     time, 24 hour notation, same as %H:%M:%S 18:06:15
%r     time, am/pm notation, same as %I:%M:%S %p 06:06:15 PM

Formatting
%n     newline character
%t     tab character
%%     percent character

Less common formats
%s    number of seconds since the Epoch, UTC
%c     national date and time representation
%+    national date and time representation
%x     national date representation
%X     national time representation
%G     year with century, starting on first Monday where week has 4 or more days.
%g    year without century, starting on first Monday where week has 4 or more days.
%V     week number of the year, starting on first Monday where week has 4 or more days.