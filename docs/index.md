---
title: "tutor: Interactive tutorials for R"
---

## Overview

The **tutor** package makes it easy to turn any [R Markdown](http://rmarkdown.rstudio.com) document into an interactive tutorial. Tutorials consist of content along with interactive components for checking and reinforcing understanding. Tutorials can include any or all of the following:

1. Narrative, figures, illustrations, and equations.

2. Code exercises (R code chunks that users can edit and execute directly).

3. Quiz questions.

4. Videos (supported services include YouTube and Vimeo).

5. Interactive Shiny applets.

To create a tutorial, just use `library(tutor)` within your Rmd file to activate tutorial mode, then use the `exercise = TRUE` attribute to turn code chunks into exercises. Users can edit and execute the R code and see the results right within their browser.

For example, here's a very simple tutorial:

<div id="hellotutor"></div>
<script type="text/javascript">loadSnippet('hellotutor')</script>

This is what the running tutorial document looks like after the user has entered their answer:

<img src="images/hello.png"  width="879" height="279" style="border: solid 1px #cccccc;"/>


## Getting Started

### Installation

1. Install the development version of the **tutor** package from GitHub as follows:

    ```r
    devtools::install_github("rstudio/tutor", auth_token = "33cdbf9d899fe6eff5022e67e21f08964f7c7b19")
    ```

2. Install the current [RStudio Daily Build](https://dailies.rstudio.com) (v1.0.114 or higher) as it includes tools for easily running and previewing tutorials.

### Creating a Tutorial

A tutorial is just a standard R Markdown document that has three additional attributes:

1. Loads the **tutor** package.
2. Includes one or more interactive components (exercises, quiz questions, etc.).
3. Uses the `runtime: shiny_prerendered` directive in the YAML header.

There is one other requirement related to R code chunks that contain exercises or quiz questions: they must have a unique chunk label. For example, this chunk is labeled `addition`:

<div id="addition"></div>
<script type="text/javascript">loadSnippet('addition')</script>

This requirement exists to ensure that a stable identifier is associated with each interactive component. This in turn makes it possible to save and restore user work as well as facilitates aggregation and reporting on responses.

The `runtime: shiny_prerendered` element included in the YAML hints at the underlying implementation of tutorials: they are simply Shiny applications which use an R Markdown document as their user-interface rather than the traditional `ui.R` file.

You can copy and paste the simple "Hello, Tutor!" example from above to get started creating your own tutorials.

### Running Tutorials

Tutorials are Shiny applications that are run using the `rmarkdown::run` function rather than the `shiny::runApp` function. For example:

```r
rmarkdown::run("tutorial.Rmd")
```

If your tutorial is included within an R package you can also run it via the `tutor::run_tutorial` function. For example, you can run a live version of the "Hello, Tutor" example provided above with:

```r
tutor::run_tutorial("hello", package = "tutor")
```

## Exercises

Exercises are interactive R code chunks that allow readers to directly execute R code and see it's results:

<img src="images/exercises.png" width=770 height=922 style="border: solid 1px #cccccc;">


Exercises can include hints or solutions as well as custom checking code to provide feedback on user answers. The [Exercises](exercises.html) page includes a more in depth discussion of exercies and their various available options and behaviors.

## Questions

You can include one or more multiple-choice quiz questions within a tutorial to help verify that readers understand the concepts presented. Questions can either have a single or multiple correct answers. 

Include a question by calling the `question` function within an R code chunk:

<div id="question"></div>
<script type="text/javascript">loadSnippet('question')</script>

Here's what the above question would look like within a tutorial:

<img src="images/question.png" width=729 height=227>

The [Questions](quesiton.html) page includes additional information on using questions within tutorials.

## Including Videos

You can include videos published on either [YouTube](https://www.youtube.com) or [Vimeo](https://vimeo.com) within a tutorial using the standard markdown image syntax. Note that any valid YouTube or Vimeo URL will work. For example, the following are all valid examples of video embedding:

<div id="videos"></div>
<script type="text/javascript">loadSnippet('videos', 'markdown')</script>

### Video Size

Videos are responsively displayed at 100% of their container's width (with height automatically determined based on a 16x9 aspect ratio). You can change this behavior by adding attributes to the markdown where you reference the video.

You can specify an alternate percentage for the video's width or an alternate fixed width and height. For example:

<div id="videos-size"></div>
<script type="text/javascript">loadSnippet('videos-size', 'markdown')</script>

## Shiny Applets

The **tutor** package uses `runtime: shiny_prerendered` to turn regular R Markdown documents into live tutorials. Since tutorials are Shiny applications at their core, it's also possible to add other forms of interactivity using Shiny (e.g. for teaching a statistical concept interactively). 

The basic technique is to add a `context="server"` attribute to code chunks that are part of the Shiny server as opposed to UI definition. For example:

<div id="shiny"></div>
<script type="text/javascript">loadSnippet('shiny')</script>

You can learn more by reading the [Prerendered Shiny Documents](http://rmarkdown.rstudio.com/authoring_shiny_prerendered.html) article on the R Markdown website.


## External Resources

You may wish to include external resources (images, videos, CSS, etc.) within your tutorial documents. Since the tutorial will be deployed as a Shiny applications, you need to ensure that these resources are placed within one of several directories which are reachable by the Shiny web server:

<table>
<thead>
<tr class="header">
<th>Directory</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>images/</code></td>
<td>Image files (e.g. PNG, JPEG, etc.)</td>
</tr>
<tr class="even">
<td><code>css/</code></td>
<td>CSS stylesheets</td>
</tr>
<tr class="odd">
<td><code>js/</code></td>
<td>JavaScript scripts</td>
</tr>
<tr class="even">
<td><code>www/</code></td>
<td>Any other files (e.g. downloadable datasets)</td>
</tr>
</tbody>
</table>

The reason that all files within the directory of the main Rmd can't be referenced from within the web document is that you may not want all files within your tutorial's directory to be downloadable by end users. By restricting the files which can be referenced to the above directories you can control which files are downloadable and which are not.

## Tutorial Formats

You aren't limited to the default `html_document` format when creating tutorials. Here's an example of embedding a tutorial within a `slidy_presentation`:

<img src="images/slidy.png" width="650" height="474" style="border: solid 1px #cccccc;"/>

You can run a live version of this example with:

```r
tutor::run_tutorial("slidy", package = "tutor")
```

You can use the **tutor** package with any R Markdown format that:

1. Inherits from the [`html_document_base`](https://www.rdocumentation.org/packages/rmarkdown/topics/html_document_base) format (this includes [`html_document`](http://rmarkdown.rstudio.com/html_document_format.html), [`ioslides_presentation`](http://rmarkdown.rstudio.com/ioslides_presentation_format.html), [`slidy_presentation`](http://rmarkdown.rstudio.com/slidy_presentation_format.html), and many others).

2. Is marked as `boostrap_compatible`. This is a parameter of [`html_document_base`](https://www.rdocumentation.org/packages/rmarkdown/topics/html_document_base) which indicates that it's safe to inject [Booststrap](http://getbootstrap.com/) CSS into the document.

## Deployment

Tutorials can be deployed all of the same ways that Shiny applications can, including running locally on an end-user's machine or running on a Shiny Server or hosting service like shinyapps.io. The easist way to deploy a tutorial is to include it within an R package and have users run it directly via the `tutor::run_tutorial` function.

The [Deployment](deployment.html) page includes an in-depth discussion of the various deployment options as well as some special considerations resources, concurrent usage, and security which come into play when deploying tutorials on a server.


