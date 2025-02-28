The Measure Information window provides the user with details on a
selected measure; its primary intent is to educate users unfamiliar with
the scorecard. All variable text is selectable (but not editable) so the
user can copy it.

## Accessing

The Dashboard tab's scorecard grid contains a link to the window for
each measure (categories are excluded); the link is an icon left-aligned
in the measure title column, occupying the space used to indent the
measure title. When clicked, the window appears and the default tab's
video pre-loads but does not start playing. No selectors are provided
within the window itself; the selections made on the Dashboard (such as
the selected date and Performance Level) are used within the window.

## Sections

The window contains four sections: Grid, Details, Calculations, and
Videos. Each focuses on a specific aspect of educating the user.

### Grid

This section, comprising the top portion of the window, displays a
scorecard grid restricted to the category of the selected measure and
the selected measure itself. The scorecard grid should provide the same
functionality as that presented in the main window, with the exception
that it should not present the links to the Measure Information window.

### Details

This section describes various aspects of the measure:

1.  Definition (manual metadata): What the measure is
2.  Perspective (manual metadata): Which side (Home Plan or Host Plan)
    the measure applies to
3.  Class (manual metadata): The focus of the measure
4.  Scoring Type (based on points formula): How points are allocated
    (e.g. Linear, All or Nothing, All or Linear)
5.  Direction (based on points formula): Whether the score being "Lower
    is better" or "Higher is better" (i.e. receives more points with a
    lower or higher score, respectively)
6.  Impact Note (manual metadata): The impact of the scenario the
    measure evaluates
7.  Related Measures (manual metadata): Those measures related to this
    one, usually because they share one or more inputs
8.  Related FlexReports (manual metadata): Those FlexReports useful in
    examining records that may fall under the scenario the measure
    evaluates

### Calculations

This section displays the score formula for the measure including the
appropriate input values. Each input may have multiple values, one for
each date used, as in the case where the user selected quarter-to-date
and the measure uses month-to-date daily inputs.

- When an input has multiple values, each value is displayed as
  "\[{input title} {m/d/yyyy}\] {input value}" (e.g. "\[Home SF Inv 31+
  Days (Volume) 1/31/2014\] 1.000000"). The values for each input are
  grouped together in the formula in order of date ascending.
- When an input has a single value, the value is displayed as "\[{input
  title}\] {input value}" (e.g. "\[Home SF Inv 31+ Days (Volume)\]
  1.000000").

Parentheses are used only when necessary to accommodate order of
operations.

### Videos

The remaining bulk of the window consists of a set of educational videos
navigated by tabs. The tabs separate the videos into "Overview"
(selected by default) and "Flowchart". Each video allows the user to
play/pause and restart, as well as providing them a scrub bar to move to
a certain time in the video. If the user switches tabs while a video is
playing, the video pauses. If the user closes the window, any video
playing stops, and going into the window again resets the time on all
videos to the beginning.

If the user clicks play on a video that is not yet ready, a loading
indicator displays until the video starts to play. If the user switches
tabs while a video is loading, the video continues to load but does not
start playing.

Because the current videos do not contain information on the new
adjustment messages, the following text appears below the videos
regardless of the tab selected: "Note: Adjustment messages are now
included in the adjustment measures."

## Mockup

<figure>
<img src="Mockup_-_Measure_Information_window.png"
title="File:Mockup_-_Measure_Information_window.png" />
<figcaption><a
href="File:Mockup_-_Measure_Information_window.png">File:Mockup_-_Measure_Information_window.png</a></figcaption>
</figure>