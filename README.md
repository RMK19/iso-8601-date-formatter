# ISO 8601: The only date format worth using

[![Image: Status of most recent build and test attempt.](https://travis-ci.org/boredzo/iso-8601-date-formatter.png?branch=master)](https://travis-ci.org/boredzo/iso-8601-date-formatter)
[![Image: Test code coverage percentage.](https://coveralls.io/repos/boredzo/iso-8601-date-formatter/badge.png?branch=master)](https://coveralls.io/r/boredzo/iso-8601-date-formatter?branch=master)

Obligatory relevant [xkcd](http://xkcd.com/):

[![Seriously now. “ISO 8601 was published on 06/05/88 and most recently amended on 12/01/04.”](http://imgs.xkcd.com/comics/iso_8601.png)](http://xkcd.com/1179/)

## How to use this code in your program

Add the source files to your project.

### Parsing

Create an ISO 8601 date formatter, then call `[formatter dateFromString:myString]`. The method will return either an NSDate or `nil`.

There are a total of six parser methods. The one that contains the actual parser is `-[ISO8601DateFormatter dateComponentsFromString:timeZone:range:]`. The other five are based on this one.

The “`outTimeZone`” parameter, when not set to `NULL`, is a pointer to an `NSTimeZone *`variable. If the string specified a time zone, you'll receive the time zone object in that variable. If the string didn't specify a time zone, you'll receive `nil`.

The “`outRange`” parameter, when not set to `NULL`, is a pointer to `NSRange` storage. You will receive the range of the parsed substring in that storage.

### Unparsing

Create an ISO 8601 date formatter, then call `[formatter stringFromDate:myDate]`. The method will return a string.

The formatter has several properties that control its behavior:

* You can set the format of the resulting strings. By default, the formatter will generate calendar-date strings; your other options are week dates and ordinal dates.
* You can set a default time zone; by default, it will use `[NSTimeZone defaultTimeZone]`.
* You can enable a strict mode, wherein the formatter enforces sanity checks on the string. By default, the parser will afford you quite a bit of leeway.
* You can set whether to include the time in the string, and if so, what hour-minute separator to use (default `':'`).

## How to test that this code works

UPDATE from the year 2013: Conversion from the old make-based test monsters to modern OCUnit-based tests is underway. Contributions of new and old test cases will be greatly appreciated.

'make test' will perform all tests. If you want to perform only *some* tests:

### Parsing

Type 'make parser-test'. make will build the test program (testparser), then invoke testparser.sh.py to generate testparser.sh. Then make will invoke testparser.sh, which will invoke the test program with various dates.

If you don't want to use my tests, 'make testparser' will create the test program without running it. You can then invoke testparser yourself with any date you want to. If it doesn't give you the result you expected, contact me, making sure to provide me with both the input and the output.

### Unparsing

Type 'make unparser-test'. make will build the test programs, then invoke testunparser.sh. This shell script invokes each test program for -01-01 of every year from 1991 to 2010, writing the output to a file, and then runs diff -qs between that file (testunparser.out) and a file (testunparser-expected.out) containing known correct output. diff should report that the files are identical.

Three test programs are included: unparse-date, unparse-weekdate, and unparse-ordinal date. If you don't want to use my tests, you can make these test programs separately. Each takes a date specified by ISO 8601 (parsed with my own ISO 8601 parser), and outputs a string that should represent the same date.

## Notes

### Version history

This version is 0.8. Changes from 0.7:

- watchOS is now officially supported. ba5cbc3 de905a1 45ab5eb (Thanks, Neil Daniels!)
- tvOS is now officially supported. b56be72 (Thanks, Boris Bügling!)
- Carthage is now officially supported. da460bf (Thanks, Agnes Vasarhelyi!)
- Added support for millisecond precision. 065612a (Thanks, Rob Keniger!)
- Fixed bug that could insert the sign character into the middle of time zone specifications. 7b189ae 5ceb8d6 (Thanks, Hector Zarate!)
- Improved thread safety of time zone cache. 98bf9cb (Thanks, Peter Steinberger!)
- Tests now use XCTest. 3cb3921 591ca75
- API now declares nullability. 40ba01d
- Header documentation should now be parseable by Xcode. a76f42c 54136ac
- Updated link to Rick McCarty's week dates algorithm. 58f3d26 (Thanks, Christopher Bowns!)
- Begun the move to newer API names. 446b9f3 (Thanks, William Kent!)
- Maintained support for legacy runtime (i386 Mac) targets. c02cffe 01ea541 (Thanks, Mike Abdullah!)
- Fixed a build error that was introduced by SDK changes. 6d01598 (Thanks, Kyle Fuller!)
- Fixed an uninitialized variable. 1b09a6a (Thanks, Marcel Jackwerth!)

This version is 0.7. Changes from 0.6:

- Cocoa Touch is now officially supported. 4bc0a08 5d95233
- Added an Xcode project with static library targets. c847ddb 4bc0a08
- `stringFromObjectValue:` now logs to the Console when you hand it a value that isn't an NSDate. Check your Console output after upgrading—you might have had a bug all this time without knowing! 4e05978
- Added proper documentation in the header, compatible with [appledoc](http://gentlebytes.com/appledoc/). f17713f
- Fixed nonsense date components/dates being returned when the input is not a valid date string. 288f757 93bb9df 0cb6442
- Fixed a bug in unparsing where the time zone was wrong (#3). 0e355ee 8f8a2c3 (Thanks, Carl Lindberg)
- Fixed a DST bug in unparsing where the time zone offset was computed in general rather than for the date being unparsed (#6). 5665132 (Thanks, Zachary West) 9e835c8 0d36057
- Fixed AM/PM (or local equivalent) appearing in some locales on iOS devices (#15). 4bc0a08 3f697b5 249b3df (Thanks, Matthias Bauch)
- Fixed another NSDateFormatter-rewriting-formats-to-12-hour-in-some-locales bug. 7c9907f (Thanks, Carl Lindberg)
- (iOS) The class now purges its time zone cache automatically in response to a memory warning. You don't need to do that yourself, anymore, so the method to purge the caches is no longer public. 138e186 3224b32 a9b6973 3f3c3b8
- Added support for strings that specify a fraction of a second. 1d3194b 0b0b1ea (Thanks, Luke Redpath)
- Added support for non-ASCII time separator characters. 47d5035
- Added support for a time zone separator. 8198f1b 3bd0c07 17c15ed (Thanks to Daniel Tull for the suggestion.)
- Fixed the parser's response to being passed `nil`. 3c12abc 4980ee6
- Fixed (I think) #5: dates being formatted with DST applied when they shouldn't. 7368fe8
- Fixed `stringFromObjectValue:` to behave as NSFormatter's documentation says it should. 3846f80 9754438
- Added real unit tests based on OCUnit. c847ddb aa1c2cb daf3cc9 9029991 d92aeb6 b412326 (Thanks, Luke Redpath) 45af6bd cfedd75 af0b93c 6a7fd88 9e835c8 9e835c8 0d36057 7368fe8 4bc0a08 3f697b5 8f8a2c3 51c1130 e888681 13a872d f49193c 10ed166 d5fdf51 4980ee6 3846f80 288f757 93bb9df daa45fb 2d61bd0 9f35c4a 8f95099 d62777a 138e186 2a63718
- Fixed range computation for strings where the date portion ends with a 'Z'. 8f95099 efa095e 808e8c4
- Fixed some formats with missing hyphens being wrongly accepted when strict mode is on. 9f35c4a c4e0f15
- Fixed week date strings not having a 'T' between date and time. 7b6fd9f 6a1b66b
- Fixed week date strings having wrong time zones. 9f6af7c
- Strings that specify intervals (as specified by ISO 8601) are now explicitly rejected, at least for now (#20). 6d539db 51c1130 bf6a2db 6aef760 (Thanks, Blake Watters)
- `ISO8601DefaultTimeSeparatorCharacter` is now declared as `const`. I hope none of you were relying on changing that. db9877c
- Upgraded this README to Markdown. 103f666 fbd34b2 ebbf65a c0b9609
- Added the above xkcd comic. fbd34b2 63ba50a da3ed65
- Some light modernization (#25, #28). cdec3e9 8df32fd bb7c5c9

Changes in 0.6 from 0.5:

* When not set to strict parsing, allow a space before the time-zone specification, for greater compatibility with NSDate output (`description`) and input (`dateWithString:`). 27603efc8a77
* Bug fix: We no longer try to format the formatter. 83415de9f527
* Bug fix: Hours are now zero-padded correctly ([#4](https://bitbucket.org/boredzo/iso-8601-parser-unparser/issue/4/time-zones-are-emitted-without-leading)). a5608e189ebe af0c6b397428
* Fixed many various compiler warnings. Some fixes contributed by Sparks. 8be3d6f7c6f2 78ae31de2170 68dc351e9fdb e7ea87db8621 873f499ae6db
* Now 75 times faster. 6a023812bd2b 05dc35d6b505 3b2225e0ce8c d59720ad015a 10f526956963 297b8dae4095 796be11aa596 cadf0f0c8199 61d2959c6921
* iOS users can now tell the ISO 8601 date formatter class to drop its caches (which you should do when you receive a memory warning). 2bb1725914b1
* Added a couple of command-line options to the calendar-format-unparser test tool. c644aadb2b14
* The parser test tool now displays parsed dates in GMT rather than the local time zone. 788c1455ecb1
* Added a test tool to test unparsing with the time included. a89a9a8b3d61
* You can now “make analysis” to run the Clang Static Analyzer on the date formatter. b3dd33841f42
* The test tools are now built using Clang. 0723d3aa6596

Changes in 0.5 from 0.4:

* Rewrote as an NSFormatter subclass using NSCalendar.
  * Making it a formatter makes it much easier to use with Bindings.
  * Using NSCalendar means we're no longer using NSCalendarDate, which Apple has said they will deprecate at some point.
* Fixed a bug in week date generation: One subtraction could give a negative result, which was a problem because my implementation of the algorithm used unsigned integers. I've changed it to use signed integers, so the result truly is negative now. I also added a test case for this.

Changes in 0.4 from 0.3:

* Added the ability to use a time separator other than ':'.

Changes in 0.3 from 0.2:

* Colin Barrett noticed that I used `%m` instead of `%M` when creating the time strings. Oops.
* Colin also noticed that I had the `?:` in `ISO8601DateStringWithTime:` the wrong way around. Oops again.

Changes in 0.2 from 0.1:

* The unparser is new. The  has been munged to allow both components together, 
* The parser has not changed.

## Implementation details
### Parsing

Whitespace before a date, and anything after a date, is ignored. Thus, "    T23 and all's well" is a valid date for the purpose of this method. (Yes, T23 is a valid ISO 8601 date. It means 23:00:00, or 11 PM.)

All of the frills of ISO 8601 are supported, except for extended dates (years longer than 4 digits). Specifically, you can use week-based dates (2006-W2 for the second week of 2006), ordinal dates (2006-365 for December 31), decimal minutes (11:30.5 == 11:30:30), and decimal seconds (11:30:10.5). All methods of specifying a time zone are supported.

ISO 8601 leaves quite a bit up to the parties exchanging dates. I hope I've chosen reasonable defaults. For example (note that I'm writing this on 2006-02-24):

* If the month or month and date are missing, 1 is assumed. "2006" == "2006-01-01".
* If the year or year and month are missing, the current ones are assumed. "--02-01" == "2006-02-01". "---28" == "2006-02-28".
* In the case of week-based dates, with  the day missing, this implementation returns the first day of that week: 2006-W1 is 2006-01-01, 2006-W2 is 2006-01-08, etc.
* For any date without a time, midnight on that date is used.
* ISO 8601 permits the choice of either T0 or T24 for midnight. This implementation uses T0. T24 will get you T0 on the following day.
* If no time-zone is specified, local time (as returned by [NSTimeZone localTimeZone]) is used.

None of the above defaults apply if you're using the NSDateComponents-based API.

When a date is parsed that has a year but no century, this implementation adds the current century.

The implementation is tolerant of out-of-range numbers. For example, "2005-13-40T24:62:89" == 1:02 AM on 2006-02-10. Notice that the month (13 > 12), date (40 > 31), hour (24 > 23), minute (62 > 59), and second (89 > 59) are all out-of-range.

As mentioned above, there is a "strict" mode that enforces sanity checks. In particular, the date must be the entire contents of the string, and numbers are range-checked. If you have any suggestions on how to make this mode more strict, please file an enhancement request in the Issues section.

### Unparsing

I use [Rick McCarty's algorithm for converting calendar dates to week dates](http://lachy.id.au/dev/script/examples/datetime/ISOwdALG.txt), slightly tweaked.

## Bugs

### Parsing

* This method won't extract a date from just anywhere in a string, only immediately after the start of the string (or any leading whitespace). There are two solutions: either require you to invoke the parser on a string that is only an ISO 8601 date, with nothing before or after (bad for parsing purposes), or make the parser able to find an ISO 8601 date as a substring. I won't do the first one, and barring a patch, I probably won't do the second one either.

* Date ranges (also specified by ISO 8601) are not supported; this method will only return one date. To handle ranges would require at least one more method.

* There is no method to analyze a date string and tell you what was found in it (year, month, week, day, ordinal day, etc.). Feel free to submit a patch.

## Contributing

This project adheres to [the Contributor Covenant, version 1.4](CODE OF CONDUCT.md). Please read that before deciding whether you are willing to contribute.

You're welcome to work on any open bug in the issue tracker, but we do have some that are [up-for-grabs](https://github.com/boredzo/iso-8601-date-formatter/issues?q=is%3Aissue+is%3Aopen+label%3Aup-for-grabs) that are particularly easy or potentially interesting.

Please don't break compatibility with old OS versions (OS X 10.7/iOS 4) until work begins on the 1.0 release. That won't happen until 0.9 is done, and starting 1.0-only work (e.g., ARCification) early will lead to conflicts.

The tests use ARC, but the formatter itself does not ([yet](https://github.com/boredzo/iso-8601-date-formatter/issues/21)). Please be sure to add the needed `release` or `autorelease` messages when adding code that creates objects.

Adding test cases is highly encouraged, especially if adding new functionality. Ideally, please test a comprehensive set of:

- success cases
- failure cases
- exception (programmer error) cases: even if something isn't supported, the behavior should still be reliable and preferably diagnostically helpful

Travis CI will run the tests automatically when you submit a pull request, and a PR with failing tests will not be accepted.

Contributions consisting of nothing but test cases are welcome. If one/some of those cases fail, please include fixes, or at least, submit an issue report for the failure(s) and surround the failing tests with:

    #if FIXED_(issue number)
    - (void) test_thingThatDoesntWorkButShould {
        ⋮
    }
    #endif FIXED_(issue number)

As far as code style, mostly, please try to remain consistent with the existing code. Please do add nullability annotations (`ISO8601_NULLABLE`/`ISO8601_NONNULL`) and `const` to new code whenever applicable.

## Copyright

This code is copyright 2006–2016 Peter Hosey. It is under the BSD license; see LICENSE.txt for the full text of the license.
