![](https://github.com/roncampbell/IRE2018/blob/master/GatoR.png?raw=true)

# Intro to R

<code>R</code> is a powerful open-source programming and statistical language. Scientists, statisticians and, increasingly, journalists are using it to find answers in mountains of data. The language itself, known as "base R", can do quite a bit. But computer scientists have built more than 12,000 add-on tools or <code>packages</code> atop base R, vastly expanding its capabilities. As a result, people turn to R for everything from analyzing multi-year medical studies to preparing charts and drawing maps.

The most popular of these packages is actually a suite of about 20 tools known as the <code>tidyverse</code>. It was developed largely by Hadley Wickham, chief scientist at <code>R Studio</code>, the graphical user interface for R. The tidyverse includes tools for importing, manipulating, editing and visualizing data; we'll be using it extensively in this class.

Before we go further, let's cover a few rules of the road. As a programming language, R is very powerful. You can do a lot of work quickly. The tradeoff is a steep learning curve. While there usually are two or three (or more) ways to do the same task in R, there are an infinite number of ways to do it wrong. Spelling and capitalization are extremely important; the strictest copy editor you ever met was easy going compared with R.

Since R is open-source, it is free. You can download R and most packages at [CRAN](https://cran.r-project.org/), the Comprehensive R Archive Network. Installing packages is a two-step process. If you were to install the tidyverse, for example, you would enter the following commands at the console. Remarks to the right, separated by hash signs, are comments, which the computer ignores.

    > install.packages("tidyverse")     # command to install package

The crack IRE-NICAR staff has already installed R, R Studio and the tidyverse on the computers we'll be using in this class. But whenever you start R, you must load a package in memory. Here's how you do that:

    > library(tidyverse)      # notice no quote mark

We're going to be creating several tables (known in R as data frames), and of course we'll be naming them. We use an arrow-like thing called an <code>assignment operator</code> to do that. Here it is: <code><-</code>. You'll be using it a lot, and there's a shortcut: In Windows, type Alt plus the minus sign. In Mac, type Option plus the minus sign.

We'll also be writing multi-line formulas, joining each line with an odd device called -- don't ask why -- a pipe. It looks like this: <code>%>%</code>. And yes, there's a shortcut: In Windows, type Shift-Control-M; in Mac, type Shift-Command-M.
