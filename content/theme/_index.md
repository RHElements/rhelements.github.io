+++
title = "Theme"
description = ""
date = 2018-08-31T14:02:31-04:00
weight = 20
draft = false
bref = ""
toc = true
+++


## Guidelines for building a RHElement

1. A RHElement should be easy to understand. If it's challenging for rocket scientists, can it be split up into simpler pieces?
2. A component can only be 1 of 2 types: content or container. 
    - If content, then it has typography styles
    - If container, then it has surface styles and padding, no typography styles!
3. Context agnostic
    - A RHElement should “just work” when you drop it onto any page (provided the proper polyfills are there). It should have ALL the styles it needs





## Color naming conventions

A UI uses color to convey:

 - Feedback: Error and success states
 - Information: Charts, graphs, and wayfinding elements
 - Hierarchy: Showing structured order through color and typography

Common colors in a design system include 1–3 primaries that represent your brand. If none of these work well as a link and button color, then you may have an extra color for that as well. It’s a good idea to use the same color for links and button backgrounds as it makes it easier for users to recognize interactive elements.

You’ll likely have neutrals for general UI backgrounds and borders—usually grays. And finally, you’ll have colors for states such as error, warning, and success. Group these colors to see how well they work together and refine as needed.