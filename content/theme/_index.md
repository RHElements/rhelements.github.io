+++
title = "Theme"
description = ""
date = 2018-08-31T14:02:31-04:00
weight = 20
draft = false
bref = ""
toc = true
+++


### UI Colors

A user interface uses color to convey:

 - Feedback: Error and success states
 - Information: Charts, graphs, and wayfinding elements
 - Hierarchy: Showing structured order through color and typography

We've exposed 3 color variants for this design system to represent your brand:

 - Base
 - Complement
 - Accent

These colors are used throughout RHElements, but accent is the color which should stand out the most. For example, if your brand colors are navy, orange, and medium gray, you'll want to set orange as the accent color. You'll see it appear on primary level call-to-action buttons and other elements that need to stand out.

If you are overriding these colors, you can do so my changing the CSS variables, like this:


	:root {
	  --rh-color--ui-base:               #030070;
	  --rh-color--ui-base--hover:        #010047;
	  --rh-color--ui-base--text:         #ffffff;    // what color text is most visible on this background color?
	  --rh-color--ui-base--text--hover:  #eeeeee;
	}

It's also a good idea to choose some neutral colors for general UI backgrounds and borders—usually grays. And finally, you’ll have colors for states such as error, warning, and success. Group these colors to see how well they work together and refine as needed.


Surface color encompass any “surface” (whether background or not) that are typically part of container-type elements, like cards or bands. These colors should be harmonious with your corporate style guide (if you have one), but they may not necessarily be your company’s primary brand colors. Pick colors that provide a good user experience, e.g. color contrast.

You can find a list of all the system colors here:
https://github.com/RHElements/rhelements/blob/master/elements/rh-sass/variables/_colors.scss