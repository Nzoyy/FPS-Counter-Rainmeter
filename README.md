# FPS Counter Rainmeter

[Releases](https://github.com/Nzoyy/FPS-Counter-Rainmeter/releases)

This Rainmeter skin displays the current framerate (FPS) using data from MSI Afterburner. It features a dynamic display that shows the FPS value and units only when you hover over the background, ensuring a clean and unobtrusive interface.

## Features

- **Dynamic Display**: The FPS counter and units are only visible when the mouse hovers over the background area.
- **Conditional Formatting**: Adjusts the position of the units text based on the FPS value to ensure optimal readability.
- **Customizable Appearance**: Easily modify the font, colors, and sizes to match your desktop theme.
- **MSI Afterburner Integration**: Uses the MSI Afterburner plugin to fetch real-time FPS data.

## Installation

1. **Download and Install Rainmeter**: If you haven't already, download and install Rainmeter from [rainmeter.net](https://www.rainmeter.net/).
2. **Download the Skin**: Download the latest version of the FPS Counter Rainmeter skin from the [Releases](https://github.com/Nzoyy/FPS-Counter-Rainmeter/releases) section.
3. **Install the Skin**: Double-click the `.rmskin` file to install it directly into Rainmeter.
4. **Configure MSI Afterburner**: Ensure MSI Afterburner is installed and configured correctly to provide the FPS data.

## Usage

1. **Load the Skin**: Open Rainmeter, find the FPS Counter Rainmeter skin in the list, and load it.
2. **Hover to Display**: Move your mouse over the background area to display the FPS and units.
3. **Customize**: Edit the included `.ini` files to customize the appearance and behavior of the skin.

## Configuration

- **Adjusting Position**: Modify the `X` and `Y` values in the `.ini` files to change the position of the text and background.
- **Changing Colors**: Update the `FontColor` and `Fill Color` values to match your desktop theme.
- **Font and Size**: Change the `FontFace` and `FontSize` to use different fonts and sizes.

## Example Configuration

Here is an example of the main configuration file:

```ini
[Variables]
skin.Style=Horizontal

@include=#@#variables.ini
@include2=#@#include\MeterStyles.inc

[MeasureMSIAfterburnerFramerate]
Measure=Plugin
Plugin=Plugins\MSIAfterburner.dll
DataSource=Framerate
MinValue=1
MaxValue=100000

[MeasureFramerateLength]
Measure=Calc
Formula=Clamp(MeasureMSIAfterburnerFramerate, 0, 999)
IfCondition=(MeasureMSIAfterburnerFramerate < 10)
IfTrueAction=[!SetVariable FramerateX 120]
IfCondition2=(MeasureMSIAfterburnerFramerate >= 10 && MeasureMSIAfterburnerFramerate < 100)
IfTrueAction2=[!SetVariable FramerateX 135]
IfCondition3=(MeasureMSIAfterburnerFramerate >= 100)
IfTrueAction3=[!SetVariable FramerateX 150]
DynamicVariables=1

[Background]
Meter=Shape
Shape=Rectangle 0,0,85,50,5 | Fill Color 0,0,0,1 | StrokeWidth 0
X=100
Y=30
MouseOverAction=[!SetOption Background Shape "Rectangle 0,0,85,50,5 | Fill Color 0,0,0,128 | StrokeWidth 2 | Stroke Color 255,0,0,255"][!SetOption Framerate Hidden "0"][!SetOption Units Hidden "0"][!UpdateMeter *][!Redraw]
MouseLeaveAction=[!SetOption Background Shape "Rectangle 0,0,85,50,5 | Fill Color 0,0,0,1 | StrokeWidth 0"][!SetOption Framerate Hidden "1"][!SetOption Units Hidden "1"][!UpdateMeter *][!Redraw]

[Framerate]
Meter=String
MeasureName=MeasureMSIAfterburnerFramerate
Text=%1
FontSize=18
FontFace=SF Pro Text
FontColor=0,255,0,255
StringAlign=Left
X=100
Y=30
AntiAlias=1
FontEffect=Shadow
ShadowColor=0,0,0,120
ShadowX=2
ShadowY=2
Hidden=1

[Units]
Meter=String
Text=FPS
FontSize=14
FontFace=SF Pro Text
FontColor=0,255,0,255
StringAlign=Left
X=#FramerateX#
Y=34
AntiAlias=1
FontEffect=Shadow
ShadowColor=0,0,0,120
ShadowX=1
ShadowY=1
Hidden=1
DynamicVariables=1

[UpdateVisibility]
Measure=Calc
Formula=MeasureMSIAfterburnerFramerate
IfCondition=(MeasureMSIAfterburnerFramerate > 0)
IfTrueAction=[!SetOption Framerate Hidden "0"][!SetOption Units Hidden "0"][!UpdateMeter *][!Redraw]
IfFalseAction=[!SetOption Framerate Hidden "1"][!SetOption Units Hidden "1"][!UpdateMeter *][!Redraw]
DynamicVariables=1

[Rainmeter]
Update=1000
OnRefreshAction=[!UpdateMeasure MeasureMSIAfterburnerFramerate][!UpdateMeasure MeasureFramerateLength][!UpdateMeasure UpdateVisibility][!UpdateMeter "Framerate"][!UpdateMeter "Units"][!Redraw]
