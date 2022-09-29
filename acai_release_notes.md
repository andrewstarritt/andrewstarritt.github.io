#  <span style='color:#0066ee'>ACAI Release Notes</span> &nbsp; &nbsp; &nbsp; &nbsp; ![](acai_logo.png?raw=true)

__Note__: the release notes are a work in progress - currently focusing on the
release notes for the most recent releases, and back filling the older release
notes as and when I find the time.

#### <span style='color:#0066ee'>Release Index</span>

[1.7](#v1.7)<br>
[1.6](#v1.6)<br>
[1.5](#v1.5)<br>
[1.4](#v1.4)<br>
[1.3](#v1.3)<br>
[1.2](#v1.2)<br>
[1.1](#v1.1)<br>


## <a name="v1.7"></a><span style='color:#0066ee'>1.7</span>

The introduction of the 1.7 series is due to how the ACAI client stores and
uses PV names internally.

### <span style='color:#00aa88'>1.7.3</span>

Release date: 10-08-2022

Added a new function getEnumerationIndex (string to int) function which is the
reverse of getEnumeration (int to string), i.e. it allow an enumeration state
string to be converted to in integer (or returns -1 when no match).<br>
__Note:__ the conversion is strict in that it __is__ case and white space
sensitive.

Modified the minFieldValue/maxFieldValue functions to refect the number of 
enumeration states when the field type is DBF_ENUM.

### <span style='color:#00aa88'>1.7.2</span>

Release date: 18-07-2022

Fixed up the total stuff up with the initilisation of an internal ACAI::Client
private data class.
This updated avoids segmentation faults on platforms such as CentOS Stream 8 and
Rocky Linux (however I got-away-with-it on CentOS 7).

Also now store the PV's hostname:port using a standard string (as opposed to a
traditional char buffer).

The obsolete ACAI_MAX_PVNAME_LENGTH macro has been removed.

### <span style='color:#00aa88'>1.7.1</span>

Release date: 03-07-2022

#### PV Names

The ACAI client now stores PV names in a std::string as opposed to a
plain char buffer array of size defined by PVNAME_STRINGSZ, i.e. 61.
The 61, or 60 limit after exluding the zero, is the IOC's record name
limit and does not allow for field names and/or additional filters and
qualifiers, e.g.

    RECORD_NAME.VAL[10001:20001]

Also the 60 limit does not necessarily apply to other Channel Access servers.

#### Client API

The API for putByteArray has changed from:

    bool putByteArray (void* valueArray, const unsigned int count);

to:

    bool putByteArray (const void* valueArray, const unsigned int count);

This should have no practical impact.

#### Long Strings

The acai_monitor program now take an __-l, --longstr__ option which tells the
program to treat arrays of DBF_CHAR as long strings, i.e. similar to the
caget -S option.

#### Shared Libary Version

The SHRLIB_VERSION in the Makefile is now just the major and minor
release numbers, i.e. is defined as:

    SHRLIB_VERSION = 1.7

and not as:

    SHRLIB_VERSION = 1.7.1

#### Private Header File

The acai_private_common.h header file is no longer "built" into the
&lt;top&gt;/include directory.


## <a name="v1.6"></a><span style='color:#0066ee'>1.6</span>

The introduction of the 1.6 series is due to a modification to how the
buffered callback queue is managed if/when the queue gets too large.

### <span style='color:#00aa88'>1.6.4</span>

Release date: 14-03-2022

Added two new client functions that return the minimum and maximum values
supported by the host's field type:

    double minFieldValue () const;
    double maxFieldValue () const;

Example: -32768 and +32767 for the DBF_SHORT field type.
Note: for DBF_CHAR, the range encompasses both CHAR and UCHAR,
i.e. -128 and +255 respectively.

Also added a function to provide the CA protocol version:

    static ACAI::ClientString protocolVersion ();

and aca_monitor now uses this when outputing version information.

Also fixed up function spelling typo in many comments.

Added the acai logo to the doxygen generated html files.

### <span style='color:#00aa88'>1.6.3</span>

Release date: 20-10-2021

Fixed a bug and potential segment faults when writing an array of
type DBF_STRING.

Also use a better marco name, BUILDING_ACAI_LIBRARY, to indicate
if/when we are building the shared library, as opposed to building
against the the shared library header file.

### <span style='color:#00aa88'>1.6.2</span>

Release date: 02-05-2021

The clientFieldTypeImage function (out of acai_client_types) now returns
"DBF_STRING", "DBF_SHORT" etc, as opposed to "STRING", "SHORT" etc.

Re-modified the acai_client getString function to use %e for very small or
very large numbers, and %f for regular numbers.
The threshold between use of %e or %f is driven by the precision.   

### <span style='color:#00aa88'>1.6.1</span>

Release date: 24-02-2021

Modified buffered_callbacks to check for multiple updates for a channel
and discard the updates if/when queue length exceeds threshold.

The getString method now uses the %g format, as opposed to the %f format,
for floats and doubles.
This is really sensible for large numbers.

acai_monitor option processing now allows combined "-mg" or "-gm" in lieu
of "-m -g"

Fixed a number of comment spelling/typo errors.

Release notes relocated to the andrewstarritt.github.io repository and
converted to mark-down.
Also some, but not all, release notes have been back-filled.
The README.md now links to the new release notes.

## <a name="v1.5"></a><span style='color:#0066ee'>1.5</span>

The introduction of the 1.5 series was due to a change in the behavour of
the putString function.

### <span style='color:#00aa88'>1.5.8</span>

Released: 27-08-2020

Added the following function to acai client.

     bool putByteArray (void* valueArray, const unsigned int count);

This writes a traditional byte array value to the channel.
Under the covers, this uses the DBF_CHAR field type.

### <span style='color:#00aa88'>1.5.7</span>

Released: 04-07-2020

Improved acai_monitor option processing and help message.

Made acai_private_common.h public for test_csnprintf and added suitable use at
your own peril warning - it is __*still*__ essentially private.

### <span style='color:#00aa88'>1.5.6</span>

There __was no__ 1.5.6 release - I incremented the version no. twice by mistake.

### <span style='color:#00aa88'>1.5.5</span>

Update to support MSVC, specifically define  snprintf  as \_snprintf for
Released: 14-01-2020

MSVC versions eariler than 1900. Also updated code to eliminate some MSVC
warnings.

### <span style='color:#00aa88'>1.5.4</span>

Released: 24-08-2019

Added the following function to acai client:

      static ACAI::Client* cast (void* item);

This function attempts to cast a void* to a Client* and checks the internal
magic numbers.
It returns NULL if cast fails, similar to dynamic_cast.

The client hostname is now included in acai_monitor meta data output.

Fixed the fuction *[sic]* typo.

### <span style='color:#00aa88'>1.5.3</span>

Released: 22-07-2019

Updated to keep MSVC happy, including:
 * Avoid constructor ambiguity - now use overloading as opposed to using
 default parameters - no user code change required;
 * Don't use dynamically sized stack variables;
 * Use size_t type variables where appropriate; and
 * Update code to remove warnings.

Now include stdlib.h for malloc/free when using mingw.

Updated the configure/RELEASE file to match latest generated by  makeBaseApp.pl
(out of base-7.0.2)

### <span style='color:#00aa88'>1.5.2</span>

Released: 29-04-2019

Modified the in logic in putString.
For PVs with a DBF type of DBF_CHAR, the PV must have two or more elements
in order to be deemed a long string.
A single DBF_CHAR (like the .PROC field) are not deemed a long string.

### <span style='color:#00aa88'>1.5.1</span>

Released: 30-03-2019

Modified the behavior of putString.
This will now always write the string data in long string format if the native
type if DBF_CHAR irrespective of the isLongString setting and/or PV name ending
with a '$'.
The isLongString setting now just applies to getString.

## <a name="v1.4"></a><span style='color:#0066ee'>1.4</span>

The introduction of the 1.4 series was due to re-factoring the acai version
macros into a separate acai_version.h file

### <span style='color:#00aa88'>1.4.8</span>

Released: 20-03-2019

Removed another not very help error report (when element count = 0, out
of the readSubscribeChannel function).

### <span style='color:#00aa88'>1.4.7</span>

Released: 18-03-2019

Don't report errors for zero length updates, these seem to be quite common
for transient PVs.

acai_monitor - tidied up info output and added copyright header to main file.

Fixed a few typos in the  README.md file and updated email address.

Ran doxygen -u over acai.cfg - mainly comment changes.

### <span style='color:#00aa88'>1.4.6</span>

Released: 16-12-2018

### <span style='color:#00aa88'>1.4.5</span>

Released: 02-05-2018

### <span style='color:#00aa88'>1.4.4</span>

Released: 21-04-2018

### <span style='color:#00aa88'>1.4.3</span>

Released: 08-04-2018

### <span style='color:#00aa88'>1.4.2</span>

Released: 28-02-2018

### <span style='color:#00aa88'>1.4.1</span>

Released: 18-02-2018

## <a name="v1.3"></a><span style='color:#0066ee'>1.3</span>

Re-organised directory structure

### <span style='color:#00aa88'>1.3.9</span>

Released: 14-01-2018

### <span style='color:#00aa88'>1.3.8</span>

Released: 22-10-2017

### <span style='color:#00aa88'>1.3.7</span>

Released: 21-10-2017

### <span style='color:#00aa88'>1.3.6</span>

Released: 22-10-2017

### <span style='color:#00aa88'>1.3.5</span>

Released: 16-08-2017

### <span style='color:#00aa88'>1.3.4</span>

Released: 27-07-2017

### <span style='color:#00aa88'>1.3.3</span>

Released: 23-07-2017

### <span style='color:#00aa88'>1.3.2</span>

Released: 09-07-2017

### <span style='color:#00aa88'>1.3.1</span>

Released: 08-07-2017

## <a name="v1.2"></a><span style='color:#0066ee'>1.2</span>

### <span style='color:#00aa88'>1.2.3</span>
Added to Client_Set

    bool areAllChannelsReady ();
    bool waitAllChannelsReady (const double timeOut, const double pollInterval = 0.05);

and to Abstract_Client_User

    bool areAllRegisteredChannelsReady ();
    bool waitAllRegisteredChannelsReady (const double timeOut, const double pollInterval = 0.05);


### <span style='color:#00aa88'>1.2.2</span>

Added put call back notification. The use of put callback is off by default.

For enumeration types, the upper display and control limit functions now
return number of states less one (as opposed to zero).

Function description typo fixes.

### <span style='color:#00aa88'>1.2.1</span>

Change of API

    void rawDataPointer (void* dest, const size_t size,
                         unsigned int& count) const;

became

    size_t getRawData (void* dest, const size_t size,
                       const size_t offset = 0) const;


## <a name="v1.1"></a><span style='color:#0066ee'>1.1</span>

### <span style='color:#00aa88'>1.1.4</span>

### <span style='color:#00aa88'>1.1.3</span>

### <span style='color:#00aa88'>1.1.2</span>

### <span style='color:#00aa88'>1.1.1</span>


<font size="-1">Last updated: Thu Sep 29 20:29:02 AEST 2022</font>
<br>
